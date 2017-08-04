# Bing::Ads

A Ruby client for Bing Ads API that includes a proxy to all Bing Ads API web services and abstracts low level details of authentication with OAuth.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'bing-ads'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install bing-ads

## Usage

### Campaign Management Service
#### Initialization
```ruby
options = {
  environment: :sandbox,
  authentication_token: '39b290146bea6ce975c37cfc23',
  developer_token: 'BBD37VB98',
  customer_id: '21027149',
  account_id: '5278183',
  # client_settings: { logger: LOGGER::STDOUT }
}

service = Bing::Ads::API::V11::Services::CampaignManagement.new(options)
```

#### Getting campaigns in account
```ruby
account_id = 5278183
response = service.get_campaigns_by_account_id(account_id)
```

#### Adding campaigns
```ruby
account_id = 5278183
campaigns = [
  {
    budget_type: Bing::Ads::API::V11.config.campaign_management.budget_limit_type.daily_budget_standard,
    conversion_tracking_enabled: "false",
    daily_budget: 2000,
    description: 'Amsterdam-based global campaign',
    name: '51 - Global - Chain - Mixed - N -en- Amsterdam - 100 - 26479',
    status: Bing::Ads::API::V11.config.campaign_management.campaign_status.paused,
    time_zone: Bing::Ads::API::V11.config.time_zones.amsterdam_berlin_bern_rome_stockholm_vienna
  },
  # ...
]

response = service.add_campaigns(account_id, campaigns)
```

#### Updating campaigns
```ruby
account_id = 5278183
campaigns = [
  {
    id: 813721838,
    budget_type: Bing::Ads::API::V11.config.campaign_management.budget_limit_type.daily_budget_standard,
    conversion_tracking_enabled: "false",
  },
  {
    id: 813721849,
    budget_type: Bing::Ads::API::V11.config.campaign_management.budget_limit_type.daily_budget_standard,
    conversion_tracking_enabled: "false",
  },
  # ...
]

response = service.update_campaigns(account_id, campaigns)
```

#### Deleting campaigns
```ruby
account_id = 5278183
campaign_ids_to_delete = [813721838, 813721813, 813721911]
response = service.delete_campaigns(account_id, campaign_ids_to_delete)
```

---

#### Getting ad groups in campaign
```ruby
campaign_id = 813721838
response = service.get_ad_groups_by_campaign_id(campaign_id)
```

#### Getting ad groups in campaign by ids
```ruby
campaign_id = 813721838
ad_group_ids = [9866221838, 9866221813, 9866221911]
response = service.get_ad_groups_by_ids(campaign_id, ad_group_ids)
```

#### Adding ad groups to campaign
```ruby
# You can add a maximum of 1,000 ad groups in a single call.
# Each campaign can have up to 20,000 ad groups.
campaign_id = 813721838
ad_groups = [
  {
    status: Bing::Ads::API::V11.config.campaign_management.ad_group_status.paused,
    ad_rotation: Bing::Ads::API::V11.config.campaign_management.ad_rotation.optimize_for_clicks, # optional
    ad_distribution: Bing::Ads::API::V11.config.campaign_management.ad_distribution.search, # required
    bidding_scheme: Bing::Ads::API::V11.config.campaign_management.inherit_from_parent, # optional
    content_match_bid: 100, # optional
    language: Bing::Ads::API::V11.config.languages.english,
    name: 'H=WHotelAmsterdam&AG=1723812002',
    native_bid_adjustment: -50, # optional (-100 to 900)
    remarketing_targeting_setting: Bing::Ads::API::V11.config.campaign_management.remarketing_target_setting.bid_only, # optional
    search_bid: 100, # optional
    start_date: '5/7/2017',
    end_date: '31/12/2020'
  },
  # ...
  # https://msdn.microsoft.com/en-us/library/bing-ads-campaign-management-adgroup.aspx
]

response = service.add_ad_groups(campaign_id, ad_groups)
```

#### Updating ad groups in campaign
```ruby
# You can add a maximum of 1,000 ad groups in a single call.
# Each campaign can have up to 20,000 ad groups.
campaign_id = 813721838
ad_groups = [
  {
    id: 9866221838,
    status: Bing::Ads::API::V11.config.campaign_management.ad_group_status.active
  },
  # ...
]

response = service.update_ad_groups(campaign_id, ad_groups)
```

---

#### Getting ads in ad group
```ruby
ad_group_id = 9866221838
response = service.get_ads_by_ad_group_id(ad_group_id)
```

#### Getting ads in ad group by ids
```ruby
ad_group_id = 9866221838
ad_ids = [454873284248, 454873284249, 454873284250]
response = service.get_ads_by_ids(ad_group_id, ad_ids)
```

#### Adding ads to ad group
```ruby
# You can add a maximum of 50 ads in an ad group.
ad_group_id = 9866221838
expanded_text_ads = [
  {
    path_1: 'Amsterdam',
    path_2: 'Hotels',
    text: 'Compare over 150 booking sites! Find guaranteed low hotel rates.',
    title_part_1: 'Hotels in Amsterdam',
    title_part_2: 'Best Deals. Book now',
    final_urls: [
      'http://www.findhotel.net/Places/Amsterdam.htm'
    ]
  },
  # ...
  # Expanded text ads
  # https://msdn.microsoft.com/en-us/library/bing-ads-campaign-management-expandedtextad.aspx
  # For other ad types:
  # https://msdn.microsoft.com/en-us/library/bing-ads-campaign-management-ad.aspx
]

# Returns IDs of ads added to ad group
# { ad_ids: [], partial_errors: [] }
response = service.add_ads(ad_group_id, expanded_text_ads)
```

#### Updating ads in ad group
```ruby
ad_group_id = 9866221838
updated_expanded_text_ads = [
  {
    id: 454873284248,
    final_urls: [
      'https://www.findhotel.net/Places/Amsterdam.htm'
    ]
  },
  # ...
]

response = service.update_ads(ad_group_id, updated_expanded_text_ads)
```

---

#### Getting keywords in ad group
```ruby
ad_group_id = 9866221838
response = service.get_keywords_by_ad_group_id(ad_group_id)
```

#### Getting keywords in ad group by ids
```ruby
ad_group_id = 9866221838
keyword_ids = [234873284248, 234873284249, 234873284250]
response = service.get_keywords_by_ids(ad_group_id, keyword_ids)
```

#### Adding keywords to ad group
```ruby
ad_group_id = 9866221838
keywords = [
  {
    bidding_scheme: Bing::Ads::API::V11.config.campaign_management.inherit_from_parent,
    bid: 5,
    # optional, ad final urls used if this is not set
    final_urls: [
      'https://www.findhotel.net/Places/Amsterdam.htm?attrs=pet-friendly'
    ],
    match_type: Bing::Ads::API::V11.config.campaign_management.match_types.exact, # also broad, content, phrase
    status: Bing::Ads::API::V11.config.campaign_management.keyword_statuses.active,
    text: 'Pet-friendly Hotels in Amsterdam'
  },
  # ...
  # https://msdn.microsoft.com/en-us/library/bing-ads-campaign-management-keyword.aspx
]

# Returns IDs of keywords added to ad group
# { keyword_ids: [], partial_errors: [] }
response = service.add_keywords(ad_group_id, keywords)
```

#### Updating keywords in ad group
```ruby
ad_group_id = 9866221838
updated_keywords = [
  {
    id: 234873284248,
    # updated attributes
  },
  # ...
]

response = service.update_keywords(ad_group_id, updated_keywords)
```

### Bulk Service
Not yet supported

### Customer Management Service
Not yet supported

### Reporting Service
Not yet supported. Use: https://github.com/FindHotel/bing-ads-reporting

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake spec` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.
To install this gem onto your local machine, run `bundle exec rake install`.

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/FindHotel/bing-ads.