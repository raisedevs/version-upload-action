# version-upload-action

Raise.dev's GitHub Action for uploading `Version`s and their binary data to the Raise.dev Console.

## Usage

**Note: unless a Raise.dev cofounder has invited you to join the Raise.dev Console, you can't use this (yet).**

Once you've signed in and had your `User` enabled, add something like this to your GitHub Actions workflow:

```yaml
steps:
- uses: raisedevs/version-upload-action@main
  id: version-upload
  if: github.ref == 'refs/heads/main' || startsWith(github.ref, 'refs/tags/')
  with:
    account: raisedevs
    firmware: firmware
    binary: .pio/build/esp32dev/firmware.bin
```

And if you wish to use the outputs, add something like:

```yaml
- name: Output Version details
  run: |
    echo Version Name: ${{ steps.version-upload.outputs.name }}
    echo Version Show URL: ${{ steps.version-upload.outputs.show-url }}
    echo Version Binary URL: ${{ steps.version-upload.outputs.binary-url }}
```

If you haven't already, add the relevant `Account` and `Firmware` in the Raise.dev Console and connect the GitHub App to your GitHub Repository for your `Firmware`.

If you haven't set up a GitHub Actions workflow yet, don't worry!

Check out the [workflow in raisedevs/raise-dev-library](https://github.com/raisedevs/raise-dev-library/blob/main/.github/workflows/build.yml) for an example of how to use PlatformIO in GitHub Actions.

Note that you should use `pio run` instead of `pio ci` for building a `Firmware` GitHub repository.

## Status

Alpha and in active development.

## Contact

[Mike McQuaid](mailto:mike@raise.dev)

## License

Licensed under the [MIT License](https://en.wikipedia.org/wiki/MIT_License).
The full license text is available in [LICENSE.txt](https://github.com/raisedevs/version-upload-action/blob/master/LICENSE.txt).
