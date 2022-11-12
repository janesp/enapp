# ENAPP Schema

The ENAPP schema specifies the data elements (structure, not content), which are required for energy availability reporting and communication. The [ENAPP schema](enapp-schema.pdf) is specified in entity-relationship notation.

## Static Information

**Provider**, **region** and **building** data changes rarely during availability reporting phases. This data is collected before reporting phases start.

### Provider

A provider of energy in any form, as specified by `Type`. Providers of electrical energy in the current context. See [register of providers of electrical energy](https://www.strom.ch/de/service/verzeichnis-verteilnetzbetreiber).

A provider has an internal `ID` and a `Name`.

An `Identifier` is used as a reference to the official provider register.

```
CREATE TABLE `provider` (
	`providerID` INT NOT NULL AUTO_INCREMENT,
	`providerName` TEXT(100) NOT NULL,
	`providerIdentifier` INT NOT NULL UNIQUE,
	`providerType` INT,
	PRIMARY KEY (`providerID`)
);
```

### Region

A region is a service area as defined by the responsible provider. A region can be independently turned off and on (such as a transformer service area). A region provides energy to a number of **buildings**.

A region is owned by a **provider** and has an internal `ID` and a `Name`.

A region can reference an **unavailability plan**.

```
CREATE TABLE `region` (
	`regionID` INT NOT NULL AUTO_INCREMENT,
	`regionName` TEXT(100) NOT NULL,
	`providerID` INT NOT NULL,
	`planID` INT NOT NULL,
	PRIMARY KEY (`regionID`)
);

ALTER TABLE `region` ADD CONSTRAINT `region_fk0` FOREIGN KEY (`providerID`) REFERENCES `provider`(`providerID`);

ALTER TABLE `region` ADD CONSTRAINT `region_fk1` FOREIGN KEY (`planID`) REFERENCES `unavailabilityPlan`(`planID`);
```

### Building

A building is an object within the service area of a region.

A building has an internal `ID` and a `Name`.

A building has an associated `Address`, `Postalcode` and `City`, and it references the **region** in which it is located.

In addition, the `EGID` references the official building register.

```
CREATE TABLE `building` (
	`buildingID` INT NOT NULL AUTO_INCREMENT,
	`buildingName` TEXT(100) NOT NULL,
	`buildingAddress` TEXT(100) NOT NULL,
	`buildingPostalcode` INT NOT NULL,
	`buildingCity` TEXT(100) NOT NULL,
	`regionID` INT NOT NULL,
	`EGID` INT,
	PRIMARY KEY (`buildingID`)
);

ALTER TABLE `building` ADD CONSTRAINT `building_fk0` FOREIGN KEY (`regionID`) REFERENCES `region`(`regionID`);
```

## Semi Dynamic Information

Person related data changes more frequently than **regions** and **buildungs** throgh registrations with the platform and for specific building addresses.

### Person

An ndividual who is reliablay registered with the platform. This individual can obtain energy availability information for a building addresses.

A person has an internal `ID`, a `firstName`, a `lastName` and a `phoneMobile`. A registered person is identified through its mobile number.

```
CREATE TABLE `person` (
	`personID` INT NOT NULL AUTO_INCREMENT,
	`firstName` VARCHAR(50) NOT NULL,
	`lastName` VARCHAR(50) NOT NULL,
	`phoneMobile` INT NOT NULL,
	`email` VARCHAR(50),
	PRIMARY KEY (`personID`)
);
```

### Person/Building

A registration of a specific person for a specific building address.

```
CREATE TABLE `personBuilding` (
	`personID` INT NOT NULL,
	`buildingID` INT NOT NULL,
	`registrationDate` DATETIME NOT NULL,
	PRIMARY KEY (`personID`,`buildingID`)
);

ALTER TABLE `personBuilding` ADD CONSTRAINT `personBuilding_fk0` FOREIGN KEY (`personID`) REFERENCES `person`(`personID`);

ALTER TABLE `personBuilding` ADD CONSTRAINT `personBuilding_fk1` FOREIGN KEY (`buildingID`) REFERENCES `building`(`buildingID`);
```

## Dynamic Information

Unavailability data changes frequently during the reporting phase.

### Unavaliability Plan

An unavailability plan is defined by the responsible provider and specifies periods with off and on dates and times. An availability plan can be referenced by multiple regions.

A unavailability plan has an internal `ID` and a `Name`.

```
CREATE TABLE `unavailabilityPlan` (
	`planID` INT NOT NULL AUTO_INCREMENT,
	`planName` TEXT(100) NOT NULL,
	`providerID` INT NOT NULL,
	PRIMARY KEY (`planID`)
);

ALTER TABLE `unavailabilityPlan` ADD CONSTRAINT `unavailabilityPlan_fk0` FOREIGN KEY (`providerID`) REFERENCES `provider`(`providerID`);
```

### Unavailability Period

An unavailability period defines a specific `Statt` and `End` date and time with in a given plan.

```
CREATE TABLE `unavailabilityPeriod` (
	`periodID` INT NOT NULL AUTO_INCREMENT,
	`planID` INT NOT NULL,
	`unavailabilityStart` DATETIME NOT NULL,
	`unavailabilityEnd` DATETIME NOT NULL,
	PRIMARY KEY (`periodID`)
);

ALTER TABLE `unavailabilityPeriod` ADD CONSTRAINT `unavailabilityPeriod_fk0` FOREIGN KEY (`planID`) REFERENCES `unavailabilityPlan`(`planID`);
```
