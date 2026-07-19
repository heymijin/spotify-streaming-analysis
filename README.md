# Spotify Streaming Analysis

A PySpark and Spark SQL project that turns Spotify listening-history files into queryable datasets for listening-pattern analysis.

## Why I built it

I wanted to understand what happens behind an analytics result: how raw JSON files are combined, cleaned, enriched, joined, stored, and queried before a chart or summary can be produced.

## Data scope

The original analysis processed approximately **13,500 streaming events** from multiple Spotify export files. The private source data included timestamps, device information, account identifiers, and IP addresses, so it is intentionally excluded from this portfolio repository.

This version includes **500 synthetic streaming events** with the same core analytical structure. It allows the workflow to be understood and adapted without exposing personal records.

| Dataset | Purpose | Key fields |
| --- | --- | --- |
| Streaming history | Event-level listening activity | endTime, artistName, trackName, msPlayed |
| Library | Track and album enrichment | artist, album, track, URI |
| Genre lookup | API-derived categorization | track URI, artist URI, genres |

## Pipeline

```text
Spotify JSON exports
        ↓
PySpark ingestion and union
        ↓
Schema checks and unique track key
        ↓
Time and duration transformations
        ↓
Library and genre enrichment
        ↓
Parquet storage in HDFS
        ↓
Spark SQL analysis
```

## Processing logic

- Combined multiple streaming-history JSON files into one Spark DataFrame.
- Created a track-level key from artist and track names.
- Split timestamps into date and time fields and converted milliseconds into listening minutes.
- Joined streaming events with saved-library metadata and API-derived genre information.
- Stored cleaned and enriched datasets as Parquet in HDFS.
- Filtered very short plays and long-form content before music-focused analysis.
- Aggregated listening behavior by artist, track, date, and time of day.

## Scale and filtering

The original private run contained approximately:

- **13,552** streaming events before music-focused filtering
- **11,160** events after excluding plays shorter than two minutes
- **11,061** events after separating content longer than fifteen minutes

These figures describe the original project run; the source rows are not included.

## Repository structure

```text
process/
  streaming_analysis.ipynb       # Code-first notebook
  streaming_analysis.zpln        # Zeppelin notebook with outputs and runtime metadata removed
sample_data/
  StreamingHistory0.json         # Synthetic event sample
  StreamingHistory1.json
  YourLibrary.json               # Synthetic library metadata
  genre.csv                      # Synthetic genre lookup
```

## Tools

PySpark · Spark SQL · Apache Zeppelin · HDFS · Parquet · Python

## Privacy note

The original Spotify export, account identifier, IP addresses, device metadata, and exact event-level listening history are not published. The sample data is synthetic, and the Zeppelin copy retains code while excluding execution outputs and runtime metadata.
