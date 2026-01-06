# Secure and open files

A single-page, client-side tool to secure (encrypt) and open (decrypt) files in the browser. Files and passphrases never leave the device.

## Features
- Client-side AES-256-GCM encryption with PBKDF2 (SHA-256).
- No uploads or network calls (CSP blocks outbound requests).
- Simple 4-step workflow for non-technical users.
- English/French UI with a language toggle (default: French).
- Outputs `.secure` files with embedded metadata for the original filename.
- File size guard to avoid heavy in-browser memory usage.

## How it works (high level)
1. Choose Secure or Open.
2. Select a file.
3. Enter a strong passphrase (5-7 random words recommended).
4. Download the resulting file.

## File format
The output is a custom binary container identified by the `SEC01` magic header:

```
[SEC01][version][iterations][salt][iv][nameLen][name][ciphertext]
```

Notes:
- The original filename is stored in plaintext in the header.
- `ciphertext` includes the AES-GCM authentication tag.

## Security notes
- Always use a strong passphrase and share it via a separate channel.
- The filename is not encrypted; avoid sensitive names.
- If hosting publicly, ensure only trusted admins can modify the HTML.

## Usage
Open `index.html` in a modern browser, or serve it over HTTPS from a static host.


## Configuration (index.html)
Key settings are in the script near the top:
- `PBKDF2_ITERATIONS` (fixed): 310,000
- `MAX_FILE_BYTES`: 200 MB (default limit)
- `MAGIC`: `SEC01`
- Output extension: `.secure`
- Fallback filename: `secure-file.bin`

## Language
- Default language is French.
- Selection is stored in `localStorage`.

## License
MIT License.
