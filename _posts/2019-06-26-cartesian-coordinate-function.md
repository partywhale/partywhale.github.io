---
layout: post
title:  "A Cartesian Celestial Coordinate Function in Python"
date:   2019-06-26 20:27:00 -0700
categories:
  - python
  - astronomy
---
As an addendum to my recent post on [converting equatorial celestial coordinates to Cartesian coordinates](/astronomy/2019/06/22/converting-equatorial-to-cartesian.html) (x, y, z), I wrote a small Python function to demonstrate. It takes right ascension in hours, minutes, and seconds; declination in degrees, hours, and seconds; and distance. It returns x y z coordinates as a tuple (but it would be easy to turn into another data type).

{% highlight python %}
import math

# Convert sexigesimal degrees to decimal degrees
def sexigesimalToDecimalDegrees(degrees, minutes, seconds):
    decimalDegrees = degrees + (minutes / 60) + (seconds / 3600)
    return decimalDegrees

# Converts right ascension, declination, and distance to x y z coordinates
def cartesianCoordinates(rhours, rminutes, rseconds, ddegrees, dminutes, dseconds, distance):
    alpha = math.radians(sexigesimalToDecimalDegrees((rhours * 15), (rminutes * 15), (rseconds * 15)))
    delta = math.radians(sexigesimalToDecimalDegrees(ddegrees, dminutes, dseconds))
    x = round((distance * math.cos(delta) * math.cos(alpha)), 3)
    y = round((distance * math.cos(delta) * math.sin(alpha)), 3)
    z = round((distance * math.sin(delta)), 3)
    return (x, y, z)
{% endhighlight %}

A quick test, again utilizing Aldebaran:

{% highlight python %}
# Right ascension: 4h 35m 55.23907s
# Declination: 16° 30' 33.4885''
# Distance: 20.0 pc
aldebaran = cartesianCoordinates(4, 35, 55.23907, 16, 30, 33.4885, 20.0)
print(aldebaran)
{% endhighlight %}

When we run the code, our Python shell spits out

> (6.878, 17.899, 5.683)

Next we'll need to write a program to utilize this function. I have two ideas in mind: a script to parse and reformat a CSV or Excel doc containing star data, and a webapp to allow anyone to convert the coordinates of a star they're curious about for... fun. But I'll leave that for another time.
