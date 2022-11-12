# Data Delivery

## Providers

Energy providers are loaded from the [official energy provider register](https://www.strom.ch/de/service/verzeichnis-verteilnetzbetreiber).

## Regions

Service area regions must be made available upfront by responsibile energy **providers** through a list of references to **buildings** within the service area.

## Buildings

Building information is loaded from the [official building address register](https://www.cadastre.ch/de/services/service/registry/building.html).

## Unavailability Plans

Unavailability plans are made available frequently by energy **providers** for the **regions** for which they are responsibile.

The data is uploaded in denormalized form based on a [availability report template](enapp-availabilityreport-template.xlsx), where every line item represents one specific unavailability period.