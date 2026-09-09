PROJECT := ThreeOneOSFive.xcodeproj
SCHEME := 3105
CONFIGURATION := Release
SDK := iphoneos
BUILD_DIR := build
DERIVED_DATA := $(BUILD_DIR)/DerivedData
ARCHIVE_PATH := $(BUILD_DIR)/$(SCHEME).xcarchive
EXPORT_PATH := $(BUILD_DIR)/ipa
EXPORT_OPTIONS_PLIST ?= ExportOptions.plist
APP_PATH := $(DERIVED_DATA)/Build/Products/$(CONFIGURATION)-iphoneos/$(SCHEME).app

.DEFAULT_GOAL := ipa-unsigned
.PHONY: archive ipa build ipa-unsigned clean

archive:
	xcodebuild \
		-project "$(PROJECT)" \
		-scheme "$(SCHEME)" \
		-configuration "$(CONFIGURATION)" \
		-sdk "$(SDK)" \
		-archivePath "$(ARCHIVE_PATH)" \
		archive

ipa: archive
	@test -f "$(EXPORT_OPTIONS_PLIST)" || (echo "Missing $(EXPORT_OPTIONS_PLIST). Set EXPORT_OPTIONS_PLIST=... or create the file first."; exit 1)
	rm -rf "$(EXPORT_PATH)"
	xcodebuild \
		-exportArchive \
		-archivePath "$(ARCHIVE_PATH)" \
		-exportPath "$(EXPORT_PATH)" \
		-exportOptionsPlist "$(EXPORT_OPTIONS_PLIST)"
	@echo "IPA exported to $(EXPORT_PATH)"

build:
	xcodebuild \
		-project "$(PROJECT)" \
		-scheme "$(SCHEME)" \
		-configuration "$(CONFIGURATION)" \
		-sdk "$(SDK)" \
		-derivedDataPath "$(DERIVED_DATA)" \
		build

ipa-unsigned: build
	rm -rf "$(BUILD_DIR)/Payload" "$(BUILD_DIR)/$(SCHEME)-unsigned.ipa"
	mkdir -p "$(BUILD_DIR)/Payload"
	cp -R "$(APP_PATH)" "$(BUILD_DIR)/Payload/"
	cd "$(BUILD_DIR)" && zip -qry "$(SCHEME)-unsigned.ipa" Payload
	rm -rf "$(BUILD_DIR)/Payload"
	@echo "Unsigned IPA created at $(BUILD_DIR)/$(SCHEME)-unsigned.ipa"

clean:
	rm -rf "$(BUILD_DIR)"