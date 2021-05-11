# LinkedIn Ad Analytics ([docs](https://fivetran-dbt-linkedin.netlify.app/#!/overview))

This package models LinkedIn Ad Analytics data from [Fivetran's connector](https://fivetran.com/docs/applications/linkedin-ads). It uses data in the format described by [this ERD](https://fivetran.com/docs/applications/linkedin-ads#schemainformation).

The main focus of the package is to transform the core ad object tables into analytics-ready models, including an 'ad adapter' model that can be easily unioned in to other ad platform packages to get a single view. This is especially easy using our [Ad Reporting package](https://github.com/fivetran/dbt_ad_reporting).

## Models

This package contains transformation models, designed to work simultaneously with our [LinkedIn Ad Analytics source package](https://github.com/fivetran/dbt_linkedin_source) and our [multi-platform Ad Reporting package](https://github.com/fivetran/dbt_ad_reporting). A dependency on the source package is declared in this package's `packages.yml` file, so it will automatically download when you run `dbt deps`. The primary outputs of this package are described below.

| **model**                          | **description**                                                                                                        |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| [linkedin__ad_adapter](models/linkedin__ad_adapter.sql)               | Each record represents the daily ad performance of each creative, including information about the used UTM parameters. |
| [linkedin__account_ad_report](models/linkedin__account_ad_report.sql)        | Each record represents the daily ad performance of each account.                                                       |
| [linkedin__campaign_ad_report](models/linkedin__campaign_ad_report.sql)       | Each record represents the daily ad performance of each campaign.                                                      |
| [linkedin__campaign_group_ad_report](models/linkedin__campaign_group_ad_report.sql) | Each record represents the daily ad performance of each campaign group.                                                |

## Installation Instructions
Check [dbt Hub](https://hub.getdbt.com/) for the latest installation instructions, or [read the dbt docs](https://docs.getdbt.com/docs/package-management) for more information on installing packages.

## Configuration
By default, this package will look for your LinkedIn Ad Analytics data in the `linkedin_ads` schema of your [target database](https://docs.getdbt.com/docs/running-a-dbt-project/using-the-command-line-interface/configure-your-profile). If this is not where your LinkedIn Ad Analytics data is, please add the following configuration to your `dbt_project.yml` file:

```yml
# dbt_project.yml

...
config-version: 2

vars:
    linkedin_database: your_database_name
    linkedin_schema: your_schema_name

```
For additional configurations for the source models, please visit the [LinkedIn Ad Analytics source package](https://github.com/fivetran/dbt_linkedin_source).

### Switching to Local Currency
Additionally, the package allows you to select whether you want to add in costs in USD or the local currency of the ad. By default, the package uses USD. If you would like to have costs in the local currency, add the following variable to your `dbt_project.yml` file:

```yml
# dbt_project.yml

...
config-version: 2

vars:
    linkedin__use_local_currency: True
```

### Passing Through Additional Metrics
By default, this package will select `clicks`, `impressions`, and `costs` from the source `ad_analytics_by_creative` table to store into the `linkedin__ad_adapter` model. If you would like to pass through additional metrics to the ad adapter model, add the following configuration to your `dbt_project.yml` file:

```yml
# dbt_project.yml

...
vars:
    linkedin__passthrough_metrics: ['the', 'list', 'of', 'metric', 'columns', 'to', 'include'] # from LINKEDIN_ADS.AD_ANALYTICS_BY_CREATIVE
```

### Changing the Build Schema
By default this package will build the LinkedIn Ad Analytics staging models within a schema titled (<target_schema> + `_stg_linkedin`) and the LinkedIn Ad Analytics final models within a schema titled (<target_schema> + `_linkedin`) in your target database. If this is not where you would like your modeled LinkedIn data to be written to, add the following configuration to your `dbt_project.yml` file:

```yml
# dbt_project.yml

...
models:
    linkedin:
      +schema: my_new_schema_name # leave blank for just the target_schema
    linkedin_source:
      +schema: my_new_schema_name # leave blank for just the target_schema
```

## Contributions
Additional contributions to this package are very welcome! Please create issues
or open PRs against `master`. Check out 
[this post](https://discourse.getdbt.com/t/contributing-to-a-dbt-package/657) 
on the best workflow for contributing to a package.

## Database Support
This package has been tested on BigQuery, Snowflake and Redshift.

## Resources:
- Provide [feedback](https://www.surveymonkey.com/r/DQ7K7WW) on our existing dbt packages or what you'd like to see next
- Have questions, feedback, or need help? Book a time during our office hours [using Calendly](https://calendly.com/fivetran-solutions-team/fivetran-solutions-team-office-hours) or email us at solutions@fivetran.com
- Find all of Fivetran's pre-built dbt packages in our [dbt hub](https://hub.getdbt.com/fivetran/)
- Learn how to orchestrate [dbt transformations with Fivetran](https://fivetran.com/docs/transformations/dbt)
- Learn more about Fivetran overall [in our docs](https://fivetran.com/docs)
- Check out [Fivetran's blog](https://fivetran.com/blog)
- Learn more about dbt [in the dbt docs](https://docs.getdbt.com/docs/introduction)
- Check out [Discourse](https://discourse.getdbt.com/) for commonly asked questions and answers
- Join the [chat](http://slack.getdbt.com/) on Slack for live discussions and support
- Find [dbt events](https://events.getdbt.com) near you
- Check out [the dbt blog](https://blog.getdbt.com/) for the latest news on dbt's development and best practices
