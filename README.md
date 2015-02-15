Java Steam API
=========

[![Build Status](https://travis-ci.org/go-ive/steam-api.svg?branch=master)](https://travis-ci.org/go-ive/steam-api)&nbsp;
[![Coverage Status](https://coveralls.io/repos/go-ive/steam-api/badge.svg?branch=master)](https://coveralls.io/r/go-ive/steam-api?branch=master)&nbsp;
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.github.go-ive/steam-api/badge.svg)](https://maven-badges.herokuapp.com/maven-central/com.github.go-ive/steam-api)

A Java library to retrieve data from Valves Steam platform. It uses the [Steam Storefront API](https://wiki.teamfortress.com/wiki/User:RJackson/StorefrontAPI), which is not inteded for public use and may change anytime (it already happened). I try to react as quickly as possible when this happens and release a new compatible version, but features might be changed or missing.

## Usage

```java
package com.github.goive.steamapidemo;

import java.util.List;

import com.github.goive.steamapi.SteamApi;
import com.github.goive.steamapi.SteamApiImpl;
import com.github.goive.steamapi.data.Category;
import com.github.goive.steamapi.data.Price;
import com.github.goive.steamapi.data.SteamApp;
import com.github.goive.steamapi.enums.CountryCode;

public class DemoApp {

    public static void main(String[] args) {
        SteamApi steamApi = new SteamApiImpl();
        steamApi.setCountryCode(CountryCode.US);

        Long appIdOfHalfLife = 70L;

        System.out.println("Retrieving single game for id " + appIdOfHalfLife + "...");

        SteamApp steamApp = steamApi.retrieveData(appIdOfHalfLife);
        String name = steamApp.getName();

        if (steamApp.isFreeToPlay()) {
            System.out.println(name + " is free to play!");
        } else {
            Price price = steamApp.getPrice();

            StringBuilder sb = new StringBuilder();
            sb.append(name);
            sb.append(" is currently at ");
            sb.append(price.getCurrency());
            sb.append(" ");
            sb.append(price.getFinalPrice());
            sb.append(" with a ");
            sb.append(price.getDiscountPercent());
            sb.append("% discount from ");
            sb.append(price.getCurrency());
            sb.append(" ");
            sb.append(price.getInitialPrice());

            System.out.println(sb.toString());
        }

        System.out.println(name + " is listed in these categories:");
        List<Category> categories = steamApp.getCategories();
        categories.stream().forEach((category) -> {
            System.out.println(" - " + category.getDescription());
        });

        System.out.println("The game is available in the following languages: "
            + steamApp.getSupportedLanguages().toString());

        System.out.println(name + " was released at " + steamApp.getReleaseDate());

        System.out.println("Here's some more info about the game:\n" + steamApp.getAboutTheGame());

        System.out.println(name + " is available for the following platforms: ");
        if (steamApp.isAvailableForLinux()) {
            System.out.println(" - Linux");
        }
        if (steamApp.isAvailableForWindows()) {
            System.out.println(" - Windows");
        }
        if (steamApp.isAvailableForMac()) {
            System.out.println(" - Mac");
        }

        System.out.println("For more functionality take a look at the javadoc of the SteamApp.class.");
    }
}
```

This retrieves almost all available data for the given Steam ID ~~or list of IDs,~~ including prices and discounts.

## Maven Dependency

```xml
<dependency>
    <groupId>com.github.go-ive</groupId>
    <artifactId>steam-api</artifactId>
    <version>2.1.2</version>
</dependency>
```

## Gradle Dependency

```gradle
dependencies {
    compile 'com.github.go-ive:steam-api:2.1.2'
}
```

## Direct Download

Or download the JAR directly from [Maven Central](http://search.maven.org/remotecontent?filepath=com/github/go-ive/steam-api/2.1.2/steam-api-2.1.2.jar).

## Nightly Snapshot Build

If you are interested in the version currently in the master branch, you can use the SNAPSHOT version. I do not recommend this for production use as this build is changing.

```xml
<dependency>
    <groupId>com.github.go-ive</groupId>
    <artifactId>steam-api</artifactId>
    <version>2.1.3-SNAPSHOT</version>
</dependency>
```
