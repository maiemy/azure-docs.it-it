---
title: Impostare uno stile mappa usando le mappe di Azure Android SDK | Mappe Microsoft Azure
description: Vengono illustrati due modi per impostare lo stile di una mappa. Per modificare lo stile, vedere come usare le mappe di Azure Android SDK nel file di layout o nella classe Activity.
author: anastasia-ms
ms.author: v-stharr
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 15dbe7d30652d0ace78bca4dc053757d57361c1a
ms.sourcegitcommit: 4064234b1b4be79c411ef677569f29ae73e78731
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/28/2020
ms.locfileid: "92895308"
---
# <a name="set-map-style-using-azure-maps-android-sdk"></a>Impostare lo stile della mappa usando le mappe di Azure Android SDK

Questo articolo illustra due modi per impostare gli stili della mappa usando le mappe di Azure Android SDK. Mappe di Azure include sei stili di mappe diversi tra cui scegliere. Per altre informazioni sugli stili di mappa supportati, vedere [stili di mappa supportati in mappe di Azure](./supported-map-styles.md).


## <a name="prerequisites"></a>Prerequisiti

Per completare il processo in questo articolo, è necessario installare [Azure Maps Android SDK](./how-to-use-android-map-control-library.md) per caricare una mappa.


## <a name="set-map-style-in-the-layout"></a>Impostare lo stile della mappa nel layout

È possibile impostare uno stile mappa nel file di layout per la classe Activity. Modificare il **layout res > > activity_main.xml** , quindi è simile a quello riportato di seguito:

```XML
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    >

    <com.microsoft.azure.maps.mapcontrol.MapControl
        android:id="@+id/mapcontrol"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:mapcontrol_centerLat="47.602806"
        app:mapcontrol_centerLng="-122.329330"
        app:mapcontrol_zoom="12"
        app:mapcontrol_style="grayscale_dark"
        />

</FrameLayout>
```

L' `mapcontrol_style` attributo precedente imposta lo stile della mappa su **grayscale_dark** . 

<center>

![grayscale_dark stile](./media/set-android-map-styles/grayscale-dark.png)</center>

## <a name="set-map-style-in-the-activity-class"></a>Impostare lo stile della mappa nella classe Activity

Lo stile della mappa può essere impostato nella classe Activity. Copiare il frammento di codice seguente nel metodo **OnCreate ()** della `MainActivity.java` classe. Questo codice consente di impostare lo stile della mappa su **satellite_road_labels** .

```Java
mapControl.onReady(map -> {
    //Set the camera of the map.
    map.setCamera(center(47.64, -122.33), zoom(14));

    //Set the style of the map.
    map.setStyle(style(MapStyle.SATELLITE));
});
```

<center>

![Style-satellite-Road-labels](./media/set-android-map-styles/satellite-road-labels.png)</center>