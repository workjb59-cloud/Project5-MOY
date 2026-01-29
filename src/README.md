# Motorgy Used Cars Scraper

Monthly scraper for Motorgy used cars listings. It collects ad details, downloads images, uploads everything to S3, and exports an Excel file.

## Features
- Scrapes all used car listings and follows each ad detail page.
- Extracts title, year, mileage, price, monthly estimate, specifications, inspection summary, features, description, and detail URL.
- Downloads all ad images and uploads them to S3.
- Adds `s3_images_paths` column (JSON list of S3 URIs) to the dataset.
- Saves data to Excel and uploads it to S3.
- Runs automatically on the 1st of every month via GitHub Actions.

## Output
The Excel export is uploaded to:

```
s3://<S3_BUCKET>/motorgy/year=<YYYY>/month=<MM>/day=<DD>/excel_files/motorgy_used_cars_<YYYYMMDD>.xlsx
```

Images are uploaded to:

```
s3://<S3_BUCKET>/motorgy/year=<YYYY>/month=<MM>/day=<DD>/images/<ad_id>/<nn>.<ext>
```

## Data Columns
- `ad_id`
- `detail_url`
- `title`
- `year`
- `mileage`
- `price`
- `monthly_estimate`
- `specs` (dict)
- `features` (dict)
- `inspection_date`
- `inspection_summary`
- `description`
- `specs_json` (JSON string)
- `features_json` (JSON string)
- `s3_images_paths` (JSON list of S3 URIs)
- `images_count`

## Requirements
- Python 3.11+
- AWS S3 bucket

Dependencies are listed in `requirements.txt`.

## Setup (Local)
1. Create a virtual environment and install dependencies.
2. Set the required environment variables:

```
AWS_ACCESS_KEY_ID=<your-access-key>
AWS_SECRET_ACCESS_KEY=<your-secret-key>
S3_BUCKET=<your-bucket-name>
```

Optional environment variables:

```
MAX_PAGES=50
REQUEST_DELAY_SECONDS=1.0
```

3. Run the scraper:

```
python src/scrape_motorgy.py
```

The scraper uses the region `us-east-1` and does not read it from the environment.

## GitHub Actions (Monthly)
The workflow runs on the 1st of every month at 03:00 UTC.

Required GitHub Secrets:
- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`
- `S3_BUCKET`

You can also run it manually from the Actions tab.

## Notes
- The scraper includes a small delay between requests to reduce load.
- Some images may fail to download; those are skipped.
- The website structure may change over time. If fields go missing, update the selectors in `src/scrape_motorgy.py`.

## Disclaimer
Use responsibly and follow the site’s terms of service.