# Additional options

## For information about additional options, please execute the following commands.

```shell-session
cd ~/crowsnest
make help
```

## Clean configuration file&#x20;

```
make clean
```

## Rebuild binarys needed by crownest

```
make buildclean && make build
```

## Generate a Debug Report

```
make report
```

The file will be found in your `home` directory, called `report.txt`

## Additional Tools / Scripts

### What is `tools/dev-helper.sh` ?

This is a neat little script to help me (KwadFan) to debug my development. But it can be abused to debug your,possible not running or running, Crowsnest install. Feel free to play around with it. You can't destroy anything.

Because this file might change it's optional parameters, it might be a good choice, to run with as shown below:

```shell-session
tools/dev-helper.sh -h
```

As a preview, what is possible at the time this guide is written:

```shell-session
crowsnest - dev-helper.sh


	 dev-helper.sh [Options]

		-h	Prints this help.
		-V	Shows Version of dev-helper.sh
		-a	Shows all available informations gathered
		-c	Shows informations about cameras
		-o	Shows informations about used OS
		-s	Shows informations about host system
		-x	Performs tests to ensure successful install of crowsnest
		-d	Decolorize file (if output is redirected to file)

```

This should explain itself, right? :wink:
