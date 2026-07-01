# mlp_website_information_scraper

This project is a Databricks Asset Bundle that crawls business websites and uses hosted LLMs to extract structured data. It runs two production pipelines in parallel: one discovers About/Team pages and extracts contact people; the other crawls homepages, extracts business details, scores extraction quality against source records, and exports a gzipped CSV to S3. It is built for Data Axle operators who deploy and run jobs, and engineers who extend pipeline stages or extraction schemas.

## Documentation

- [User Playbook](mlp_website_information_scraper/user-playbook)
- [Developer Playbook](mlp_website_information_scraper/developer-playbook)