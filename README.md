> [!NOTE]
> This repository contains a curated GTFS dataset assembled from multiple upstream public transport data sources.
> The compilation, transformations, and original additions in this repository are licensed under CC BY 4.0.
> Underlying source data remains subject to its original licence terms and attribution requirements. See [DATA_SOURCES.md](/DATA_SOURCES.md).

# GTFS Feeds for Great Britain
### Covers all bus, coach, National Rail, ferry, Underground and metro system.

This is a collection of data-enriched, curated GTFS feeds used by [Momego](https://travelwhiz.app) and other TravelWhiz apps and websites, free for use.

GTFS feeds are generated between 22.00 and 00.00 UTC each day.

Traveline TNDS TransXChange data is used as the base, then BODS TransXChange and TfL Journey Planner TransXChange data used as supplementary data sources which replace Traveline TNDS data on an operator/route/trip basis.

## Data Sources

- **Traveline TNDS**: [https://www.travelinedata.org.uk](https://www.travelinedata.org.uk)
- **Bus Open Dats Service (BODS)**: [https://www.travelinedata.org.uk](https://www.bus-data.dft.gov.uk)
- **Transport for London Open Data:** [https://tfl.gov.uk/info-for/open-data-users/](https://tfl.gov.uk/info-for/open-data-users/)
- **NapTAN**: [https://beta-naptan.dft.gov.uk](https://beta-naptan.dft.gov.uk)
- **National Coach Dataset**: [https://data.bus-data.dft.gov.uk/coach/download](https://data.bus-data.dft.gov.uk/coach/download)
- **OpenStreetMap Contributors**: [https://www.openstreetmap.org/about](https://www.openstreetmap.org/about)

## GB Bus & Metro feeds

#### Downloads

- East Anglia: [uk-busmetro-EA.gtfs.zip](https://storage.travelwhiz.app/generated-gtfs/uk-busmetro-EA.gtfs.zip)
- East Midlands: [uk-busmetro-EM.gtfs.zip](https://storage.travelwhiz.app/generated-gtfs/uk-busmetro-EM.gtfs.zip)
- South East & London: [uk-busmetro-SE-LONDON.gtfs.zip](https://storage.travelwhiz.app/generated-gtfs/uk-busmetro-SE-LONDON.gtfs.zip)
- North East England: [uk-busmetro-NE.gtfs.zip](https://storage.travelwhiz.app/generated-gtfs/uk-busmetro-NE.gtfs.zip)
- North West England: [uk-busmetro-NW.gtfs.zip](https://storage.travelwhiz.app/generated-gtfs/uk-busmetro-NW.gtfs.zip)
- Scotland: [uk-busmetro-S.gtfs.zip](https://storage.travelwhiz.app/generated-gtfs/uk-busmetro-S.gtfs.zip)
- South West England: [uk-busmetro-SW.gtfs.zip](https://storage.travelwhiz.app/generated-gtfs/uk-busmetro-SW.gtfs.zip)
- Wales: [uk-busmetro-W.gtfs.zip](https://storage.travelwhiz.app/generated-gtfs/uk-busmetro-W.gtfs.zip)
- West Midlands: [uk-busmetro-WM.gtfs.zip](https://storage.travelwhiz.app/generated-gtfs/uk-busmetro-WM.gtfs.zip)
- Yorkshire: [uk-busmetro-Y.gtfs.zip](https://storage.travelwhiz.app/generated-gtfs/uk-busmetro-Y.gtfs.zip)
- National Coach Services: [uk-coach.gtfs.zip](https://storage.travelwhiz.app/generated-gtfs/uk-coach.gtfs.zip)

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

#### Notes

Two operators (FlixBus and Ember) have been removed as they have their own official GTFS feeds which supersedes their TNDS/NCSD versions.

#### Extended GTFS Fields
- `trips.realtime_trip_id`: a compound key to make it easier to match vehicles in BODS' live location SIRI feed to GTFS trips. See Realtime Compound Key section for details.
- `trips.txc_vehicle_journey_ref`: a reference to the original TransXChange `VehicleJourneyRef` for that trip.
- `stops.stop_bearing`: the NaPTAN `Bearing` value. A cardinal direction for that stop (supports `N`, `NE`, `E`, `SE`, `S`, `SW`, `W`, `NW`).
- `stops.stop_indicator`: the NaPTAN `Indicator` value. Usually found on stop poles in larger towns and cities.

#### Realtime Compound Key

Format:
`<AGENCY_ID>:<LINE_KEY>:<DIRECTION_ID>:<ORIGIN_STOP_ID>[:<DESTINATION_STOP_ID>]:<HHMM>`

##### Component rules:

- `AGENCY_ID`: TXC service.AgencyId, uppercased and trimmed.
- `LINE_KEY`: TXC service.PublicServiceCode normalized to uppercase alphanumeric. If it is purely numeric, leading zeroes are stripped.
- `DIRECTION_ID`: TXC service.DirectionId as a string
- `ORIGIN_STOP_ID`: The first stop’s NapTAN AtcoCode.
- `DESTINATION_STOP_ID`: The last stop’s NapTAN AtcoCode.
- `HHMM` First stop departure time if present, otherwise trip departure time. Over-midnight values are wrapped mod 24, so `25:10:00` becomes `0110`.

##### Example:

`NOC1:10:0:STOPA:STOPB:0800`

##### Important behavior:

- If `agency_id`, `line_key`, `origin_stop_id`, or `HHMM` is missing, the field is exported blank.
- This is a semi-stable matching key, not a globally unique primary key.
- For realtime matching, developers should still scope by service date separately.

## National Rail feed

#### Downloads

[gb-nationalrail.gtfs.zip](https://storage.travelwhiz.app/generated-gtfs/gb-nationalrail.gtfs.zip)

#### Features

- Agency branding (`routes.route_color` and `routes.route_text_color`) added where possible.
- Covers every National Rail service. Most bus / ferry replacement routes have been omitted.
- NaPTAN `AtcoCode` is used for `stop_id`, and `CRS` is used for `stop_code`.
- Calendars have been deduped.
- shapes.txt generated from OpenStreetMap data.

National Rail services don't fit easily into US-centric GTFS routes. Instead of distinct lines like 'Harlem Line' or 'New Haven Line' like with New York's rail system, National Rail passengers are more used to operator-name-to-destination directions such as 'ScotRail train to Glasgow Queen Street'. For this reason, National Rail routes in this GTFS feed are split into operator-specific corridors. For example:

Route ID `SR:ALO-GLQ`, Long Name: `Alloa - Glasgow Queen Street via Stirling`

Where the Route ID is a compound key comprised of the agency ID, origin CRS station code and destination CRS station code (with an optional deterministic hash at the end to avoid route ID collisions). `routes.txt` keeps `route_short_name` blank, as there's no useful information to provide in the case of National Rail.

This means that for each operator, you will have dozens, possibly hundreds of routes. This works well with journey planning systems like OpenTripPlanner. However, if you want to display a typical departure board like you would find in most transport apps e.g. `11:05 ScotRail to Glasgow Queen Street` you will need to modify the data that's displayed - instead of `ROUTE NAME > DESTINATION`, use `AGENCY NAME > DESTINATION`, like Momego (and many other apps) do.

#### Extended GTFS Fields
- `stop_times.platform_code`: Where available, the published platform number for each arrival/departure is included.
- `trips.realtime_trip_id`:
A compound key of **Service UID** and **Run Date** e.g. `U10021:20260401` to easily match GTFS trip IDs with real-time service IDs.

## Other UK Transport GTFS Feeds

If an official GTFS feed exists for a transport operator, we tend to use that instead of Traveline. For completeness, here are some links to official GTFS feeds and transport operator developer pages.

- **Ember**: [https://api.ember.to/v1/gtfs/static](https://api.ember.to/v1/gtfs/static)
- **FlixBus UK/EU**: [http://gtfs.gis.flix.tech/gtfs_generic_eu.zip](http://gtfs.gis.flix.tech/gtfs_generic_eu.zip)
- **Transport for Ireland**: [https://developer.nationaltransport.ie](https://developer.nationaltransport.ie)
- **Eurostar**: [https://transport.data.gouv.fr/datasets/eurostar-gtfs-plan-de-transport-et-temps-reel](https://transport.data.gouv.fr/datasets/eurostar-gtfs-plan-de-transport-et-temps-reel)

## Contributions Welcome ##
If you would like to see improvements to this feed, feel free to raise an issue. We want to make this the most comprehensive, data-enriched source for GB bus, metro and rail GTFS data out there.
