# xlsx-to-edn

A small Clojure application designed to transform Excel (.xlsx) hospital reports into .edn files in the context of the prais2cljs project.

## Setting up the project

Before a non-developer can use this program, one must build the uberjar of the application that is used in the `convert.sh` script.
You need to:
- get `Java` installed (jdk 1.8 or later)
- get `Leiningen` [installed](https://leiningen.org/#install)

Then run `lein uberjar` from the root directory of the project.

This should create a `target/` folder containing among other files the required `xlsx-to-edn-0.1.0-SNAPSHOT-standalone.jar` file. If the name is different, please update `convert.sh` accordingly.

### Precautions

Make sure:
- that the `.xlsx` file is in the same format as `resources/prais2020.xlsx`;
- to update the `hospitals-location.edn`file in case there are new hospitals in the project. To do that, make sure its code (e.g. `"FRE"` for Newcastle, Freeman Hospital) is unique in the whole file, and matches the one used in the prais2 project. Add the name of the hospital, its latitude and longitude.
- to enter the right number of hospitals that are reporting data in your file.

Otherwise, there will be issues while loading the new data.

### Known problems

If you use `Excel` in a country where the decimal separator is set to `,` and not `.` in the locale, this will lead to an error.

You should make sure that the Excel data you are importing are using `.` as the decimal separator.

## Usage (general)

1. Go to the root repository and check that both the `resources/` and `target/` folder exist.

2. Check that the `resources/` repository contains:
- your reporting data in a `.xlsx`format, 
- all the previous data currently displayed by the site (those from all the previous reporting periods) in a `data.edn` file. If this file doesn't exist but your data lives in `previous-data.edn`, that is also fine.

3. Execute in a terminal from the root directory `./convert.sh "your-file.xlsx" number-of-hospitals` where your-file.xlsx is the complete path to your excel file (e.g. `/home/user/project/prais2/prais2020.xlsx` if you didn't put it under `resources/`), and number-of-hospitals is the exact number of hospitals reporting data for the period.
> You may need to turn the script into an executable first by running `chmod +x convert.sh`

## Usage (developers)

If you are a developer and have `Leiningen` installed, you can go through the following steps to test the code:

1. Set up the `resources` folder so that it contains your `.xlsx` file, along with the current `data.edn` file.

2. Rename the `data.edn` file to `previous-data.edn`.

3. Execute `lein run "your-file.xlsx" number-of-hospitals`

Should you change the code, you'll need to run `lein uberjar` to create a new jar for the application. You'd also possibly need to update the `convert.sh` script as well.

## Results

A new `data.edn` file is created, compiling the data from `previous-data.edn` with the report's data, in a single map. The new data is found under the key corresponding to the maximum key in `previous-data.edn` incremented by 1. For instance, if the latest data in `previous-data.edn` is under the key :2019, the newly imported data will be found under the key :2020.


