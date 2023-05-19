# raisedevs/version-upload-action

[Raise.dev](https://raise.dev)'s GitHub Action for uploading `Version`s and their binary data to the Raise.dev Console.

## Usage

**Note: unless a Raise.dev cofounder has invited you to join the Raise.dev Console, you can't use this (yet).**

1. Sign in to the Raise.dev Console and follow the onboarding steps until it tells you to use this library.

1. Setup GitHub Actions for your preferred environment (we recommend [PlatformIO](https://platformio.org)) by adding this file to `.github/workflows/build.yml` in your repository:

   ```yaml
   # The name of the GitHub Actions workflow
   name: Build
   # When to build i.e. on pushes to main branch, pushes to pull requests, a published release
   on:
     push:
       branches:
         - main
     pull_request:
     release:
       types: [published]
   jobs:
     # The name of the GitHub Actions job
     build:
       runs-on: ubuntu-latest
       steps:
         # Checkout the Git repository
         - uses: actions/checkout@v3
         # Cache PlatformIO data to speed things up on repeated runs
         - uses: actions/cache@v3
           with:
             path: |
               ~/.cache/pip
               ~/.platformio/.cache
             key: ${{ runner.os }}-pio
         # Setup Python to install PlatformIO
         - uses: actions/setup-python@v4
           with:
             python-version: "3.9"
         # Install PlatformIO CLI
         - run: pip install --upgrade platformio
         # Run PlatformIO CLI to build firmware
         - run: pio run
         # Upload firmware to the Raise.dev Console's raisedevs Workspace and firmware Firmware
         # only on a push to the main branch or published release
         - uses: raisedevs/version-upload-action@main
           id: version-upload
           if: github.ref == 'refs/heads/main'
           with:
             workspace: raisedevs
             firmware: firmware
             binary: .pio/build/esp32/firmware.bin
         # Output the name and some URLs for debugging and quick links
         - name: Output Version details
           if: ${{ github.ref == 'refs/heads/main' || github.event_name == 'release' && github.event.action == 'published' }}
           run: |
             echo Version Name: ${{ steps.version-upload.outputs.name }}
             echo Version Show URL: ${{ steps.version-upload.outputs.show-url }}
             echo Version Binary URL: ${{ steps.version-upload.outputs.binary-url }}
   ```

1. When you're set up correctly, when you push a new commit and/or tag to your GitHub repository: it will automatically update all your `Device`s!

---

If you have any problems: contact [Mike](mailto:mike@raise.dev) and he'll help.

## Status

Alpha and in active development.

## Contact

[Mike McQuaid](mailto:mike@raise.dev)

## License

Licensed under the [MIT License](https://en.wikipedia.org/wiki/MIT_License).
The full license text is available in [LICENSE.txt](https://github.com/raisedevs/version-upload-action/blob/master/LICENSE.txt).
