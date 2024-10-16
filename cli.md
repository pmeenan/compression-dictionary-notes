# Creating dictionary-compressed responses

The response stream formats for [compression dictionary transport](https://github.com/WICG/compression-dictionary-transport) include a magic file header and a hash of the dictionary. The CLI commands below will create files in the correct format.


## dcb

Assuming the dictionary file is `dictionary.txt` and the file to be compressed is `data.text`, this will create a dcb-compressed file `data.txt.dcb` that includes the magic header and embedded dictionary hash:

```bash
echo -en '\xffDCB' > data.txt.dcb && \
openssl dgst -sha256 -binary dictionary.txt >> data.txt.dcb && \
brotli --stdout -D dictionary.txt data.txt >> data.txt.dcb
```

## dcz

Assuming the dictionary file is `dictionary.txt` and the file to be compressed is `data.text`, this will create a dcz-compressed file `data.txt.dcz` that includes the magic header and embedded dictionary hash:

```bash
echo -en '\x5e\x2a\x4d\x18\x20\x00\x00\x00' > data.txt.dcz && \
openssl dgst -sha256 -binary dictionary.txt >> data.txt.dcz && \
zstd -D dictionary.txt -f -o tmp.zstd data.txt && \
cat tmp.zstd >> data.txt.dcz
```
