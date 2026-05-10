+++
date = '2026-05-10T18:13:57+03:00'
draft = false
title = 'Affine Cipher in Go'
+++

![Affine cipher](../../images/affine-cipher.png)

# Introduction

As part of the `Exercism` Go track I've written an implementation of
the [Affine cipher](https://en.wikipedia.org/wiki/Affine_cipher).

The installation and usage instructions can be found at my GitHub
profile [here](https://github.com/sdobrau/affinecipher).

Here is an example encrypt and decrypt. Note that the ciphertext is split
in chunks of 5 for harder cryptanalysis:
```sh
[affinecipher]$ go run main.go -a 3 -b 5 -s "Hello world" -e
armmv tvemo
[affinecipher]$ go run main.go -a 3 -b 5 -s "armmv tvemo" -d
helloworld
```

# Theory
## Encryption

The encryption function is:

```text
E(x) = (ai + b) mod m
```

Where:

- `i` is the letter's index from `0` to the length of the alphabet - 1.
- `m` is the length of the alphabet.
  For the Latin alphabet `m` is `26`.
- `a` and `b` are integers which make up the encryption key.

Values `a` and `m` must be _coprime_ (or, _relatively prime_) for
automatic decryption to succeed, i.e., they have number `1` as their
only common factor (more information can be found in the [Wikipedia
article about coprime integers][coprime-integers]).

Ciphertext is written out in groups of fixed length separated by
space, the traditional group size being `5` letters.  This is to make
it harder to guess encrypted text based on word boundaries.

---

For mapping letters to value we use a `map` with its key a Go `rune` and
its value an `int`:

```go
// Mapping of letters to values.
var letterMap = map[rune]int {
	'a': 0,
	'A': 0,
	'b': 1,
	'B': 1,
	'c': 2,
	'C': 2,
	// and so on...
}
```

The encode function does the following steps:
- Checks if key `a` is coprime with `m`
- Trims whitespace using `strings.ReplaceAll`
- Trims punctuation using the `\p{P}` Unicode Regexp character class and `ReplaceAll`
- For each character using `range` , applies the formula `E(x) = (ai + b) mod m` 

```go
// Encode a string using the Affine cipher.
func Encode(text string, a, b int) (string, error) {
	if !IsCoprime(a, 26) {
		return "", ErrNotCoprime
	}
	var cipherText strings.Builder
	text = strings.ReplaceAll(text," ", "") // trim whitespace

	// trim punctuation
	pattern := "[\\p{P}]"
	re := regexp.MustCompile(pattern)
	text = re.ReplaceAllString(text, "")
	
	for _, char := range text {
		_, isNotDigit := strconv.Atoi(string(char))
		if isNotDigit != nil { //if letter
			encValue := (a * letterMap[char] + b) % 26
			cipherText.WriteString(string(codeMap[encValue]))
		} else if isNotDigit == nil { // is digit, just append
			cipherText.WriteString(string(char))
		}		
	}
	
	return SplitStringIntoFivedChunks(cipherText.String()), nil
}
```

## Decryption

The decryption function is:

```text
D(y) = (a^-1)(y - b) mod m
```

Where:

- `y` is the numeric value of an encrypted letter, i.e., `y = E(x)`
- it is important to note that `a^-1` is the modular multiplicative inverse (MMI) of `a mod m`
- the modular multiplicative inverse only exists if `a` and `m` are coprime.

The MMI of `a` is `x` such that the remainder after dividing `ax` by `m` is `1`:

```text
ax mod m = 1
```

More information regarding how to find a Modular Multiplicative Inverse and what it means can be found in the [related Wikipedia article][mmi].

--- 

The decode function:
- Applies the decryption function using an opposite `code-to-letter` map:

```go
var codeMap = map[int]rune {
        0: 'a',
        1: 'b',
        2: 'c',
		// and so on...
}
```

```go
// Decode a string using the Affine cipher.
func Decode(text string, a, b int) (string, error) {
	var plainText strings.Builder
	if!IsCoprime(a, 26) {
		return "", errors.New("A and 26 are not coprime.")
	}
	text = strings.ReplaceAll(text," ", "") // trim whitespace
	
	for _, char := range text {
		// D(y) = (a^-1)(y - b) mod m
		if unicode.IsDigit(char) {
			plainText.WriteString(string(char))			
		} else {
			encryptedLetterValue := letterMap[char] // y 
			// a^-1 = FindMMIOfA(a)
			decValue := Mod26((FindMMIOfA(a) * (encryptedLetterValue - b)))
			plainText.WriteString(string(codeMap[decValue]))
		}
	}	
	return plainText.String(), nil
}
```

## Example of finding a Modular Multiplicative Inverse (MMI)

Finding MMI for `a = 15`:

- `(15 * x) mod 26 = 1`
- `(15 * 7) mod 26 = 1`, ie. `105 mod 26 = 1`
- `7` is the MMI of `15 mod 26`

--- 

Implemented in code:

```go
func FindMMIOfA(a int) int {
	i := 0
	for {
		if (a*i)%26 == 1 {
			return i
		} else {
			i++
		}
	}
}
```

[mmi]: https://en.wikipedia.org/wiki/Modular_multiplicative_inverse
[coprime-integers]: https://en.wikipedia.org/wiki/Coprime_integers


