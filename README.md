# Energy Availability Planners Platform

Welcome to our «Energy Availability Planners Platform» ENAPP, an initiative of the «Swiss Energy Data Working Group»!

We aim to make current and future availabilities of energy transparent to all affected stakeholders.

Stay tuned for more information to come!

## Availability Data

Communitcating energy availability to affected stakeholders is based on a few data sets:

- **Providers** - providers of electricity (or other forms of energy, such as gas). See [list of providers of electrical energy](https://www.strom.ch/de/service/verzeichnis-verteilnetzbetreiber).
- **Regions** - service areas as defined by their providers. Regions can be independently turned off and on (such as a transformer area). Regions provide energy to a number of **buildings**. Regions are initiatilly reported by providers to the platform as reference data.
- **Availability plans** - these plans are defined by providers and specify periods with off and on dates and times. Availability plans are applied to regions. Availability plans are periodically reported by providers to the platform for coordinated communicaion to stakeholders.
- **Persons** - individuals who are reliablay registered with the platform. For security reasons, these individuals are entitled to obtain energy availability information for a limited number of building addresses (typically their home address and those of a few relatives).

Availability data is specified in detail in the [ENAPP schema](schema/README.md).

## Data Delivery

Actual data is taken from various sources, as specified in the [data delivery section](data/README.md):

- Providers - from official register
- Regions - by energy providers (upfront)
- Buildings - from offical register
- Unavailability plans - by energy providers (frequently)
