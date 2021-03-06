# Generating the PlantUML Icons for GCP

If you would like to have customized builds and/or experiment with *PlantUML Icons for GCP*, you can generate your own distribution of icons and PUML files for local use.

## Prerequisites

To generate the PlantUML files locally, ensure the following is prerequisites have been completed:

* Python 3.6/3.7 and packages from the `requirements.txt` file.
* [Amazon Corretto 8](https://docs.aws.amazon.com/corretto/latest/corretto-8-ug/downloads-list.html) or [OpenJDK 8](https://openjdk.java.net/install/) installed and available from the command line. Newer versions may also be used but have not been tested.
* Download the latest [GCP Architecture Icons - Assets - PNG](https://cloud.google.com/icons) from here, unzip,  and copy the PNG file contents from `GCP Icons/Products and services` directory to `source/official` directory.

  the folder structure should look like this:

    ```
    ├── gcp-icons-for-plantuml
          └── source
              ├── GCPCommon.puml
              └── official
                  ├── AI and Machine Learning
                  ├── API Management
                  ├── Compute
                ...
    ```

## Configure

### Configuration File: config.yml

The `config.yml` file is used to map specific file names to GCP categories, and set  the name and parameters set for each category or individual file when running the `icon-builder.py` script. The included configuration file is based on the latest release of the [GCP Architecture Icons](https://cloud.google.com/icons).

For general categories, the `Color` attribute is set to match as closely as possible the color represented for that category. The color palettes used are in the `Defaults` section and then reference for the category, or can be overridden per-icon.

On top, each GCP service is mapped to it's primary category.

Next, install the python packages from the `requirements.txt` file. Depending upon your operating system, this may be through `apt`, `yum`, or `pip install` if using a virtual environment. The two requirements are:

- [PyYAML](https://pyyaml.org/)
- [Pillow](https://github.com/python-pillow/Pillow)

For PIP users, simply run `pip3 install -r requirements.txt` in your environment.

## Run

To verify all dependencies are met, run `icon-builder.py` with the `--check-env` parameter, and if all is good, run the script without any flags..

```bash
$ ./icon-builder.py --check-env
Prerequisites met, exiting
```

Next, run the same command without `--check-env` to create all new icons and update the `config.yml` file.

### What Happens

From a logical point of view, the following happens:

1. The `config.yml` is loaded
1. Cleanup: all files and directories from `dist` folder are deleted.
1. GCPCommon.puml and supporting PUML files are copied to `dist`.
1. All files ending in `.png` are processed in the `source/official` directory:
    * Matching files will have a `Target` name and `Color` setting applied.
    * Non-matching files be set to Uncategorized with default `Target` and `Color` settings.
1. For each file, the source PNG will be resized, preserving transparency if set.
1. A PlantUML sprite is generated.
1. In addition to single GCP services PUML files, a combined PUML file, named `all.puml`, is created for each category.
1. A markdown table with all GCP services,  image/icon, and the PUML name is generated.

## License Summary

Code is made available under the MIT license in `LICENSE-CODE`.

The compiled [Plant-UML jar](http://plantuml.com/download), `scripts/plantuml.jar`, is licensed under the MIT license in `LICENSE-CODE`.
