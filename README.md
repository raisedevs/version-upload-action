# firmware-upload-action

Raise's GitHub Action for uploading firmware to Raise.dev.

## Using

Add something like the following to a GitHub Actions workflow:

```yaml
steps:
- uses: raisedevs/firmware-upload-action@main
  id: firmware-upload
  if: github.ref == 'refs/heads/main' || startsWith(github.ref, 'refs/tags/')
  with:
    name: A Fantastic Firmware
    path: .pio/build/esp32dev/firmware.bin
```

And if you wish to use the outputs, add something like:

```yaml
- name: Output firmware URLs
  run: |
    echo Firmware URL: ${{ steps.firmware-upload.outputs.url }}
    echo Firmware Binary URL: ${{ steps.firmware-upload.outputs.binary-url }}
```
