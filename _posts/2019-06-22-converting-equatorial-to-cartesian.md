---
layout: post
title:  "Converting Equatorial Celestial Coordinates to a Cartesian System"
date:   2019-06-22 10:48:54 -0700
categories: astronomy
---
Celestial coordinates are usually given in an equatorial coordinates system, utilizing right ascension and declination, or their location relative to Earth. This is useful for finding stars in the night sky on Earth, but not from other locations. What if we were on a planet orbiting Aldebaran, and needed to find a particular star from this different vantage point? Or wanted to find the distance between two stars, rather than the distance of each star from Earth? Cartesian coordinates (x, y, z) are much more flexible for this purpose.

It’s pretty easy to convert in and out of Cartesian coordinates, meaning we can use this system to coordinate different systems. Another advantage is we can choose a different origin point by subtracting the coordinates of the star you wish to use as origin from each other star’s coordinates. If we want to model the night sky from a planet in a different star system we’ll have an easier time with universal Cartesian coordinates than with angles from the perspective of Earth.

> ![Rene Descartes](/assets/descartes.jpg "Rene Descartes")
> Rene Descartes, who invented Cartesian coordinates in 1637.

We can calculate Cartesian coordinates as long as we have right ascension (α), declination (δ), and distance. Since celestial objects are in motion, it’s also important to use a time reference (epoch). Presently, the equatorial coordinates of celestial objects are usually given according to the J2000.0 epoch, which is January 1st 2000 12:00 Terrestrial Time (about 11:59 UTC). Right ascension is usually given in hour angles, declination is usually given in sexigesimal degrees (base-60, using minutes and seconds), and distance is usually given in parsecs (pc) or light-years (ly). For instance, Aldebaran has a right ascension of 04h 35m 55.23907s, a declination of +16° 30’ 33.4885’’, and a distance of about 20.0 parsecs. We’ll need to know how to convert these measurements into a form we can use, but I’ll get into that later.

First, we’ll define our axes. Like in an equatorial system, we set the origin point to the centre of the Earth, and our xy-plane will be the celestial plane (an imaginary plane emanating from the Earth’s equator, used as the fundamental plane in the equatorial system).

* The X-axis points toward δ = 0 degrees, α = 0.0 hours. This is the vernal (March) equinox point. It points toward the sun at the time it appears to cross the celestial equator northward. The vernal point is also the origin of right ascension in the equatorial system.

* The Y-axis points toward δ = 0 degrees, α = 6.0 hours. This is 90 degrees east of X (6 hours * 15 = 90 degrees).

* The Z-axis points toward δ = +90 degrees, which is along the Earth’s north polar axis.

Now, we need to change our measurements into a form we can use. Hour angles and sexigesimal degrees (in minutes and seconds) need to be converted to decimal degrees or radians. If you’re using Excel or Python to do your math, you’ll need radians. Converting right ascension angular hours, minutes, and seconds into sexigesimal degrees, minutes and seconds is relatively simple: we just multiply each number by 15.

> ![Aldebaran](/assets/aldebaran.jpg)
> Aldebaran, a red giant star relatively close to Earth. It is one of the brightest stars in the night sky, and there is evidence it hosts a planetary system of its own. I use it as an example because it's the first star I thought of, thanks to Captain Picard's experience with Aldebaran whisky.

For example, Aldebaran’s right ascension of 04h 35m 55.23907s can be expressed in sexigesimal degrees as follows:

> 4 * 15 = 60  
> 35 * 15 = 525  
> 55.23907 * 15 = 828.58605

> 60° 525’ 828.58605’’

In the sexigesimal system, 1° = 60’ = 3600’’, or 1” = (1/3600°). If we want we can make things a little more readable.

> 60° 525’ 828.58605’’ becomes 68° 58’ 48.58605’’

But this won’t matter once we convert further, since we’ll be summing the components anyway. Our formula to convert sexigesimal degrees to decimal degrees is:

> D = d + (m / 60) + (s / 3600)

Where D is decimal degrees, d is sexigesimal degrees (e.g. 60°), m is minutes (e.g. 525’), and s is seconds (e.g. 828.58605’’). If we plug in our values for Aldebaran, the equation looks like

> D = 60 + (525 / 60) + (828.58605 / 3600)

and resolves to 68.9801627917 degrees. We need to do the same conversion with declination (Aldebaran’s declination is +16° 30’ 33.4885’’ which converts to 16.5093023611 degrees). We can convert decimal degrees to radians with this formula:

> Radians = degrees * (pi / 180)

For Aldebaran, right ascension converts to 1.2039309593 radians, and declination to 0.2881416834 radians. As alluded to earlier, Excel sine and cosine functions expect you to pass them a radian (you can convert degrees to radians with the RADIANS() function). This is also the case in many programming languages. Python’s math.cos() and math.sin() functions expect a radian (and Python will convert degrees to radians for us with the math.radians() function!). If we want to automate our Equatorial-to-Cartesian conversion, we can’t leave out this step.

The last element we need is distance. Thankfully, celestial distance is almost always expressed in parsecs (pc) or light-years (ly). We can use either of these units. If you use light-years, the unit of your Cartesian coordinates will be light-years; if you use parsecs, the unit of your coordinates will be parsecs. If you want to chart more than one celestial object, make sure you’re using one unit or the other. It’s easy to convert between the two.

> 1 pc = 3.261563777 ly

So to turn parsecs into light-years we multiply our parsecs by 3.261563777, and to turn light-years into parsecs we divide light-years by the same number. Parsec is usually the preferred distance unit among astronomers. If by chance you have distance in another unit:

> 1pc = 206264.8 AU = 30,856,775,814,913,673 metres

With our right ascension (α) and declination (δ) in radians, and our distance (d) in parsecs (or light-years), we can find our coordinates in x, y, z.

> x = d * cos(δ) * cos(α)

> y = d * cos(δ) * sin(α)

> z = d * sin(δ)

To demonstrate, let’s return to our good friend Aldebaran! As we found earlier, Aldebaran has a right ascension of 1.2039309593 radians and a declination of 0.2881416834 radians. Its distance from Earth is about 20.0 pc. Let’s plug in these values.

For x:

> x =  20.0 * cos(0.2881416834) * cos(1.2039309593)  
> x = 20.0 * 0.9587736104 * 0.3586911565  
> x = 6.878 pc

For y:

> y =  20.0 * cos(0.2881416834) * sin(1.2039309593)  
> y =  20.0 * 0.9587736104 * 0.9334562948  
> y =  17.899 pc

For z:

> z =  20.0 * sin(0.2881416834)  
> z = 20.0 * 0.2841710119  
> z = 5.683 pc

And there we have it! In sum, our new coordinate is (6.878, 17.899, 5.683), measured in parsecs. Of course, we can find the coordinates in light-years by multiplying each value by 3.261563777 (and probably rounding to a few decimal places). It would not be terribly difficult to iterate over a bunch of star systems and plot the coordinates into a three-dimensional star map.

A few more things worth mentioning:

On this scale, placing our origin point in the centre of Earth (versus the Sun) doesn’t really matter, since distances inside our solar system are so small in comparison to the distances between stars. Conventionally, the center of the Earth is used, since Earth is our usual point of reference. We could use the center of the Sun as our origin instead, and define -X as pointing to Earth, and the coordinates we produce would be virtually identical (assuming we had equatorial coordinates from the Sun, otherwise they would be absolutely identical but systematically biased from origin). The point is, we can say the Sun is our origin and it wouldn’t make a difference, so feel free to mark your origin as the Sun, Sol, or whathaveyou on any star map you create.

To put this in perspective, the distance between the Earth and the Sun is about 149,597,870.7 kilometres, or 1 AU. The distance between Earth and the nearest other star (Proxima Centauri) is 1.3012 pc, or about 268,392 AU, or 4.015*10^13 km. That’s a proportion of about 0.00000373! A specific origin only matters if we need a very high degree of precision!

That said, celestial bodies are constantly drifting (including Earth), and we’re usually looking at where an object was rather than currently is because the light takes years to reach us. The reason we use a standard reference time (J2000.0) is because we’re plotting moving bodies in reference to a system (Sun-Earth) that is also in motion with fluctuations in that motion. To really see the sky from another star system, we’d need to figure out the actual location of our objects using their velocity, accounting for the time it takes the light to reach us, and the time it takes the light to reach our new origin point. These Cartesian coordinates serve only as a reasonable approximation of location. Don’t use these coordinates to plan a trip to a nearby star system or you probably won’t get where you meant to go. These coordinates ultimately tell you where stars appeared to be, relative to Earth, at a specific date and time, but with some more information we might be able to make it more accurate to 'true' location.
