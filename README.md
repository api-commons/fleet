# Fleet and Telematics

The common fleet vocabulary -- vehicles, the drivers who drive them, the trips they make, the locations they report, the geofences around them and the inspections that keep them safe -- distilled from the APIs that already exist. Part of [Schema Commons](https://github.com/api-commons/schema-commons),
published by the [API Commons](https://apicommons.org).

Built from the providers tagged Fleet Management, Telematics, Connected Vehicles, Fleet, Vehicle Tracking, GPS Tracking, ELD in [APIs.io](https://apis.io) that
publish their own OpenAPI. Only harvested contracts count -- nothing API Evangelist
generated. Every number below is a count of providers.

## What the market already agrees on

<!-- commons:summary -->
| Concept | Expose it | Readable schema | Core properties (share) | Collection named |
| --- | ---: | ---: | --- | --- |
| vehicle | 29 | 26 | vin 69%, id 65%, make 50%, model 50% | vehicles 25, vehicle 7, assets 1 |
| driver | 8 | 7 | id 100%, name 86%, phone 71%, status 57% | drivers 7, driver 2 |
| trip | 11 | 9 | id 56% | trips 10, journeys 3, trip 1 |
| vehicle_location | 3 | 3 | latitude 100%, longitude 100%, speed 100%, timestamp 100% | gps 2, locations 1 |
| geofence | 5 | 5 | id 100%, name 100%, description 60% | zones 3, geofence 2, geofences 1 |
| inspection | 3 | 3 | id 100% | dvir 1, dvirs 1, inspection 1 |

Cohort: 85 providers with harvested OpenAPI, 29 crosswalks. Of the providers exposing a vehicle: 55% create, 86% list, 69% fetch, 31% update, 31% delete. Generated 2026-11-11 by build.py.
<!-- /commons:summary -->

Shares are of the providers with a readable resource schema for that concept. The
extended tier and the long tail of every concept are in [profile.yml](profile.yml).

## What the measurement shows

- A vehicle agrees on its VIN, make, model and id -- the most physical core in the commons: the identity of a vehicle is fixed by the vehicle, not by the software.
- A vehicle location agrees completely where it is published (latitude, longitude, speed, timestamp), but only three providers publish it as a resource; most stream it.
- Fleet is thin overall (29 providers expose vehicles): telematics platforms gate their APIs to customers and integration partners.

## Artifacts

- [openapi.yml](openapi.yml) -- OpenAPI 3.1 of the shared operations. No one serves it; it
  is the shape to map to. Lints clean against the
  [naming ruleset](https://github.com/api-commons/schema-commons/blob/main/ruleset/schema-commons-naming.yaml).
- [schema/](schema) -- JSON Schema 2020-12 per concept; each property carries
  `x-commons-tier`, `x-commons-share`, `x-commons-providers` and the spellings seen.
- [crosswalk/](crosswalk) -- one per provider, common name -> their name, with the harvested
  spec and operations each mapping was read from.
- [adopters.yml](adopters.yml) -- who exposes which concept, linked to their APIs.io page.
- [concepts.yml](concepts.yml) -- the only hand-written input: concepts, path segments and
  their qualifiers, aliases, reviewed exclusions, merged duplicates, and the decisions below.
- [census/](census) -- the cohort and the raw census the profile is built from.

## Decisions

These are calls, not measurements, recorded so they can be revisited as the method matures.

- Locations count only beneath a vehicle, asset or device; a bare `/locations` is a store or a work site in other contexts. GPS points, positions and breadcrumbs are vehicle locations.
- Journeys are trips; zones, places and landmarks are geofences; DVIRs are inspections.
- Drivers, trips, locations, geofences and inspections `require: [vehicle]`, so a delivery network's drivers or an irrigation system's zones do not count.

## Rebuild

```bash
../schema-commons/scripts/run.sh .
```

## License

Artifacts CC BY-NC-SA 4.0 ([LICENSE](LICENSE)); code Apache 2.0 ([LICENSE-CODE](LICENSE-CODE)).
