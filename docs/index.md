# Welcome to the weather station project documentation

## Project goal
The simplified goal of the project is to design and implement a weather monitoring system based on simple weather stations (nodes).
The data collected by each node is sent to a server where it is stored for further processing.
The server also host a simple website where the data can be accessed.
The project aim at developing the node hardware and software, and the server software.

The true goal is to have produced nodes that can opperate autonomously in the rought alpine climate present in my montains (cold temperature (-30 C), heavy snowfall and strong winds (> 200 kmph)).
However, before to be able to produce such nodes I feel like it would be better to have something that can opperate in nice climate (inside a house or in a city).
Thus I will first focus on building a "conventional" weather monitoring system that I will then improve up to the point where it can be used where I indtend to.

This project will be combined with another project of mine, the production of a cheap ultrasonic anemometer.
I like to fly and having a good idea of the wind is crutial.
However, anemometer are expensive and not fit for use in winter.
Ultrasonic ones are better for use in harsh environement but they are ridiculously expensive, even if their operation is relatively simple.

I do not intend to make some fancy app, pretty HW and all that bullshit. 
Just to make something to collect and visualise data in a simple and effective way.
Everything will be public and open and I will gladly accept help from whoever will want to contribute.
All in all if I end up with a thermometer in my bedroom that I can check from my phone I will bbe happy.

## Project motivation
The motivation for this project is that there is no cheap environemental sensor that could survive the harsh winter we have where I live.
This type of weather station is typicaly only used by governemental organisation or research center and they are too expensive / too large to be wide spread.
Moreover, the data from these is not openly availiable.

## Project Timeline
14/04/2020 - Official start of the project

## Project milestones
### Mark I (city system)
* [ ] Produce a first prototype of node and server (breadboard mess type of thing).
* [ ] Produce integrated node for city use (semi-integrated in a custom PCB type of thing).
* [ ] MkI of the system completed (node, server, website).

### Mark II (Improved for harsh winter climate)
* [ ] Create a autonomous power and life-support system for the node.
* [ ] Improved node HW / SW to work with new power and life-support.
* [ ] Node MkII completed (might also call it winter node)
* [ ] Survived one winter
* [ ] MkII of the system completed (node, server, website).


## Existing similar system
Here is a totaly non exhaustive list of similar projects.
All of them are intended for city usage.

### Citizen efforts

* [Hackair](https://www.hackair.eu/)
* [Luftdaten](https://luftdaten.info/fr/accueil/) (Very similar to what I try to do, exept I want a more modular system..)
* [aircarto](https://aircarto.fr/index.php) (Project seems frozen..)
* [airboxlab](https://blog.airboxlab.com/)
* [Sensebox](https://sensebox.kaufen/) (sort of for profit still..)

### For profit

* [Air things](https://www.airthings.com/)
* [HabitatMap](https://www.habitatmap.org/)
* [openAQ](https://openaq.org/#/?_k=378sfm)