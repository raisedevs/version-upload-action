# version-upload-action

Raise's GitHub Action for uploading Versions and their binary data to Raise.dev.

## Using

Add something like the following to a GitHub Actions workflow:

```yaml
steps:
- uses: raisedevs/version-upload-action@main
  id: version-upload
  if: github.ref == 'refs/heads/main' || startsWith(github.ref, 'refs/tags/')
  with:
    project: raisedevs
    firmware: raise_firmware
    path: .pio/build/esp32dev/firmware.bin
```

And if you wish to use the outputs, add something like:

```yaml
- name: Output Version details
  run: |
    echo Version Name: ${{ steps.version-upload.outputs.name }}
    echo Version Show URL: ${{ steps.version-upload.outputs.show-url }}
    echo Version Binary URL: ${{ steps.version-upload.outputs.binary-url }}
```
