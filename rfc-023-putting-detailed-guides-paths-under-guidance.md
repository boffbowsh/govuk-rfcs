# Problem

Currently detailed guides live at urls like: [https://www.gov.uk/british-forces-overseas-posting-cyprus](https://www.gov.uk/british-forces-overseas-posting-cyprus)  
This can cause problems because editors are able to create documents and claim high-profile slugs which shouldn't really be used for guidance  
Instead they should be published as https://www.gov.uk/guidance/[thing]

Note that currently manuals are also published under `/guidance` so we will need to check using url arbiter or equivalent to ensure the path can be claimed.

# Proposal

1. 

Ensuring new detailed guides are served under guidance/

  1. 

in Whitehall, adding 'guidance/' in front of the detailed\_guides#show route in routes.rb. However keeping the old route live to cover deploy time (to be removed 30mn+ after deploy). It will be the same as the current route,&nbsp;without 'as:detailed\_guides'.

  2. in Whitehall, update&nbsp;the [presenter](https://github.com/alphagov/whitehall/blob/master/app/models/registerable_edition.rb#L26-L32)for sending the paths to panopticon to reflect the changes in the paths
  3. In Panopticon, we&nbsp;might also need to [update the slug validation code](https://github.com/alphagov/govuk_content_models/blob/master/app/validators/slug_validator.rb) as it may not accept detailed\_guide artefacts with a `/` in the slug  
  
2. Ensuring the existing detailed guides are served under guidance/
  1. Panopticon migration to reslug all detailed guides and avoid creating duplicates
  2. 

Republish - run [panopticon::register\_guidance rake task](https://github.com/alphagov/whitehall/blob/master/lib/tasks/panopticon.rake#L24)&nbsp;to republish the content items of existing detailed guides.

  3. 

We need to create an equivalent rake task to republish to publishing-api

  4. 

In Whitehall, run the [rummager::reset::detailed](https://github.com/alphagov/whitehall/blob/master/lib/tasks/rummager.rake#L44) rake task

  5. 

In collections-publisher, run a data migration to update all api-urls for when detailed guidance is curated into topics

3. Redirecting old paths of existing detailed guides
  1. Several options:
    1. extract the urls to a csv and put them into router data
    2. add an "was\_previously\_under\_root" boolean to detailed\_guide model, create redirect item for each detailed guide for which it is true&nbsp;
    3. add redirects as a model so that a detailed guide has a redirects association, and the redirects are pushed to the Publishing API when a detailed guide is published.

&nbsp;
