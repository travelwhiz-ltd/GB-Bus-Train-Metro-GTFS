> [!NOTE]
> This repository contains a curated GTFS dataset assembled from multiple upstream public transport data sources.
> The compilation, transformations, and original additions in this repository are licensed under CC BY 4.0.
> Underlying source data remains subject to its original licence terms and attribution requirements. See [DATA_SOURCES.md](/DATA_SOURCES.md).

# GTFS Feeds for Great Britain

This is a collection of data-enriched, curated GTFS feeds used by [Momego](https://travelwhiz.app) and other TravelWhiz apps and websites, free for use.

GTFS feeds are generated between 22.00 and 00.00 UTC each day.

Traveline TNDS TransXChange data is used as the base, then BODS TransXChange and TfL Journey Planner TransXChange data used as supplementary data sources which replace Traveline TNDS data on an operator/route/trip basis.

- **Traveline TNDS**: [https://www.travelinedata.org.uk](https://www.travelinedata.org.uk)
- **Bus Open Dats Service (BODS)**: [[https://www.travelinedata.org.uk](https://www.bus-data.dft.gov.uk))
- **Transport for London Open Data:** [https://tfl.gov.uk/info-for/open-data-users/](https://tfl.gov.uk/info-for/open-data-users/)
- **NapTAN**: [https://beta-naptan.dft.gov.uk](https://beta-naptan.dft.gov.uk)
- **National Coach Dataset**: [https://data.bus-data.dft.gov.uk/coach/download](https://data.bus-data.dft.gov.uk/coach/download) 

## GB Bus & Metro feeds

#### Downloads

- East Anglia:
- East Midlands:
- South East & London:
- North East England:
- North West England:
- Scotland:
- South West England:
- Wales:
- West Midlands:
- Yorkshire:
- National Coach Services:

#### Features

- Route branding (`routes.route_color` and `routes.route_text_color`) added where possible.
- Sensible `routes.route_long_name` (usually 'ORIGIN - DESTINATION').
- Calendars have been deduped.
- `calendar_dates.txt` extends 10 days into the past, and 90 days into the future (using the import date as the base).
- `shapes.txt` shape locations taken directly from TransXChange sources where available; otherwise OpenStreetMap data was used to create shapes.
- Agency IDs are canonical **NOC** IDs.
- London and SE Traveline regions have been merged into one region (`SE-LONDON`) so TfL sources don't overlap.
- Glasgow Subway services now split into two logical routes 'Inner Circle' and 'Outer Circle'.
- All tram and underground/metro services have branded route colors.

#### Extended GTFS Fields
- `trips.realtime_trip_id`: a compound key to make it easier to match vehicles in BODS' live location SIRI feed to GTFS trips.
- `trips.txc_vehicle_journey_ref`: a reference to the original TransXChange `VehicleJourneyRef` for that trip.
- `stops.stop_bearing`:the NaPTAN `Bearing` value. A cardinal direction for that stop (supports `N`, `NE`, `E`, `SE`, `S`, `SW`, `W`, `NW`).
- `stops.stop_indicator`:the NaPTAN `Indicator` value. Usually found on stop poles in larger towns and cities.

## National Rail feed

#### Downloads


#### Features

- Agency branding (`routes.route_color` and `routes.route_text_color`) added where possible.
- Covers every National Rail service. Most bus / ferry replacement routes have been omitted.
- NaPTAN `AtcoCode` is used for `stop_id`, and `CRS` is used for `stop_code`.
- Calendars have been deduped.

#### Extended GTFS Fields
`stop_times.platform_code`:
Where available, the published platform code for each arrival/departure is included.

`trips.realtime_trip_id`:
A compound key of **Service UID** and **Run Date** e.g. `U10021:20260401` to easily match GTFS trip IDs with real-time service IDs.

## Contributions Welcome ##
If you would like to see improvements to this feed, feel free to raise an issue. We want to make this the most comprehensive, data-enriched source for GB bus, metro and rail GTFS data out there.
