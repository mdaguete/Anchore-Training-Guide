## Inspecting Image Content

During the analysis of container images the Anchore Engine performs deep inspection, collecting data on all artifacts in the image including files, operating system packages and software artifacts such as Ruby GEMs and Node.JS NPM modules.

The `image content` command can be used to return detailed information about the content of the container image.

`anchore-cli image content INPUT_IMAGE CONTENT_TYPE`

The INPUT_IMAGE can be specified in one of the following formats:

- Image Digest
- Image ID
- registry/repo:tag

the CONTENT_TYPE can be one of the following types:

- os: Operating System Packages
- files: All files in the image
- npm: Node.JS NPM Modules
- gem: Ruby GEMs
- java: Java Archives
- python: Python Artifacts

For example: `anchore-cli image content debain:latest files`

The CLI will output a subseet of fields from the content view, for example for `files` on the file name and size are displayed. To retrieve the full output the `--json` parameter should be passed. 

For example: `anchore-cli --json content debian:latest files`

### Next Steps

- [View security vulnerabilities in the image]()
- [Evaluate the image]() against policies you create
- Subscribe to receive [notifications]() when the image is updated, when the policy status changes, or when new vulnerabilites are detected. 

