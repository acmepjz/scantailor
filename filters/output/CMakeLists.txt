PROJECT("Output Filter")

INCLUDE_DIRECTORIES(BEFORE "${CMAKE_CURRENT_BINARY_DIR}")

SET(
	sources
	ApplyColorsDialog.cpp ApplyColorsDialog.h
	ImageViewTab.h
	TabbedImageView.cpp TabbedImageView.h
	Filter.cpp Filter.h
	OptionsWidget.cpp OptionsWidget.h
	Task.cpp Task.h
	CacheDrivenTask.cpp CacheDrivenTask.h
	OutputGenerator.cpp OutputGenerator.h
	OutputMargins.h
	Settings.cpp Settings.h
	Thumbnail.cpp Thumbnail.h
	Utils.cpp Utils.h
	Params.cpp Params.h
	BlackWhiteOptions.cpp BlackWhiteOptions.h
	ColorGrayscaleOptions.cpp ColorGrayscaleOptions.h
	RenderParams.cpp RenderParams.h
	ColorParams.cpp ColorParams.h
	OutputImageParams.cpp OutputImageParams.h
	OutputFileParams.cpp OutputFileParams.h
	OutputParams.cpp OutputParams.h
	PictureLayerProperty.cpp PictureLayerProperty.h
	PictureZonePropFactory.cpp PictureZonePropFactory.h
	PictureZonePropDialog.cpp PictureZonePropDialog.h
	PictureZoneComparator.cpp PictureZoneComparator.h
	PictureZoneEditor.cpp PictureZoneEditor.h
	OnDemandPictureZoneEditor.cpp OnDemandPictureZoneEditor.h
	FillColorProperty.cpp FillColorProperty.h
	FillZonePropFactory.cpp FillZonePropFactory.h
	FillZoneComparator.cpp FillZoneComparator.h
	FillZoneEditor.cpp FillZoneEditor.h
	ColorPickupInteraction.cpp ColorPickupInteraction.h
	DespeckleState.cpp DespeckleState.h
	DespeckleView.cpp DespeckleView.h
	DespeckleVisualization.cpp DespeckleVisualization.h
	DespeckleLevel.cpp DespeckleLevel.h
)
SOURCE_GROUP("Sources" FILES ${sources})

FILE(GLOB ui_files "ui/*.ui")
SOURCE_GROUP("UI Files" FILES ${ui_files})

QT5_WRAP_UI(ui_sources ${ui_files})

ADD_LIBRARY(output STATIC ${sources} ${ui_sources})
TARGET_LINK_LIBRARIES(output Qt5::Widgets Qt5::Xml)
ADD_DEPENDENCIES(output toplevel_ui_sources)

TRANSLATION_SOURCES(scantailor ${sources} ${ui_files})
