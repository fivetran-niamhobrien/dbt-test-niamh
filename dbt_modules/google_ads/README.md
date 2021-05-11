# Google Ads 

This package models Google Ads data from [Fivetran's connector](https://fivetran.com/docs/applications/google-ads).

The main focus of the package is to transform the core ad object tables into analytics-ready models, including an 'ad adapter' model that can be easily unioned in to other ad platform packages to get a single view.  This is especially easy using our [Ad Reporting package](https://github.com/fivetran/dbt_ad_reporting).

## Models

This package contains transformation models, designed to work simultaneously with our [Google Ads source package](https://github.com/fivetran/dbt_google_ads_source) and our [multi-platform Ad Reporting package](https://github.com/fivetran/dbt_ad_reporting). A dependency on the source package is declared in this package's `packages.yml` file, so it will automatically download when you run `dbt deps`. The primary outputs of this package are described below.

| **model**                       | **description**                                                                                                                    |
| ------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| google_ads__url_ad_adapter      | Each record represents the daily ad performance of each URL in each ad group, including information about the used UTM parameters. |
| google_ads__criteria_ad_adapter | Each record represents the daily ad performance of each criteria in each ad group.                                                 |
| google_ads__click_performance   | Each record represents a click, with a corresponding Google click ID (gclid).                                                      |

## Installation Instructions
Check [dbt Hub](https://hub.getdbt.com/) for the latest installation instructions, or [read the dbt docs](https://docs.getdbt.com/docs/package-management) for more information on installing packages.

## Configuration

By default, this package will look for your Google Ads data in the `adwords` schema of your [target database](https://docs.getdbt.com/docs/running-a-dbt-project/using-the-command-line-interface/configure-your-profile). If this is not where your Google Ads data is, please add the following configuration to your `dbt_project.yml` file:

```yml
# dbt_project.yml

...
config-version: 2

vars:
    google_ads_source:
        google_ads_schema: your_schema_name
        google_ads_database: your_database_name 
```

For additional configurations for the source models, visit the [Google Ads source package](https://github.com/fivetran/dbt_google_ads_source).

### Changing the Build Schema
By default this package will build the Google Ads staging models within a schema titled (<target_schema> + `_stg_google_ads`) and the Google Ads final models with a schema titled (<target_schema> + `_google_ads`) in your target database. If this is not where you would like your modeled Google Ads data to be written to, add the following configuration to your `dbt_project.yml` file:

```yml
# dbt_project.yml

...
models:
  google_ads:
    +schema: my_new_schema_name # leave blank for just the target_schema
  google_ads_source:
    +schema: my_new_schema_name # leave blank for just the target_schema
```

## Contributions

Additional contributions to this package are very welcome! Please create issues
or open PRs against `master`. Check out 
[this post](https://discourse.getdbt.com/t/contributing-to-a-dbt-package/657) 
on the best workflow for contributing to a package.

## Resources:
- Provide [feedback](https://www.surveymonkey.com/r/DQ7K7WW) on our existing dbt packages or what you'd like to see next
- Find all of Fivetran's pre-built dbt packages in our [dbt hub](https://hub.getdbt.com/fivetran/)
- Learn more about Fivetran [in the Fivetran docs](https://fivetran.com/docs)
- Check out [Fivetran's blog](https://fivetran.com/blog)
- Learn more about dbt [in the dbt docs](https://docs.getdbt.com/docs/introduction)
- Check out [Discourse](https://discourse.getdbt.com/) for commonly asked questions and answers
- Join the [chat](http://slack.getdbt.com/) on Slack for live discussions and support
- Find [dbt events](https://events.getdbt.com) near you
- Check out [the dbt blog](https://blog.getdbt.com/) for the latest news on dbt's development and best practices
