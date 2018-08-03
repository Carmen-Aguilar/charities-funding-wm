# Grants for West Midlands' charities at its lowest level since 2011

In XX I publish...

The code used to scrape the website is:
    #!/usr/bin/env python
    import scraperwiki
    import requests
    from lxml import html
    import re

    url = "http://grantnav.threesixtygiving.org/search?json_query=%7B%22aggs%22%3A+%7B%22currency%22%3A+%7B%22terms%22%3A+%7B%22size%22%3A+3%2C+%22field%22%3A+%22currency%22%7D%7D%2C+%22recipientOrganization%22%3A+%7B%22terms%22%3A+%7B%22size%22%3A+3%2C+%22field%22%3A+%22recipientOrganization.id_and_name%22%7D%7D%2C+%22fundingOrganization%22%3A+%7B%22terms%22%3A+%7B%22size%22%3A+3%2C+%22field%22%3A+%22fundingOrganization.id_and_name%22%7D%7D%2C+%22recipientDistrictName%22%3A+%7B%22terms%22%3A+%7B%22size%22%3A+3%2C+%22field%22%3A+%22recipientDistrictName%22%7D%7D%2C+%22recipientRegionName%22%3A+%7B%22terms%22%3A+%7B%22size%22%3A+3%2C+%22field%22%3A+%22recipientRegionName%22%7D%7D%7D%2C+%22query%22%3A+%7B%22bool%22%3A+%7B%22must%22%3A+%7B%22query_string%22%3A+%7B%22query%22%3A+%22%2A%22%2C+%22default_field%22%3A+%22_all%22%7D%7D%2C+%22filter%22%3A+%5B%7B%22bool%22%3A+%7B%22should%22%3A+%5B%5D%7D%7D%2C+%7B%22bool%22%3A+%7B%22should%22%3A+%5B%5D%7D%7D%2C+%7B%22bool%22%3A+%7B%22must%22%3A+%7B%7D%2C+%22should%22%3A+%5B%5D%7D%7D%2C+%7B%22bool%22%3A+%7B%22must%22%3A+%7B%7D%2C+%22should%22%3A+%7B%22range%22%3A+%7B%22amountAwarded%22%3A+%7B%7D%7D%7D%7D%7D%2C+%7B%22bool%22%3A+%7B%22should%22%3A+%5B%7B%22range%22%3A+%7B%22awardDate%22%3A+%7B%22gte%22%3A+%222010%7C%7C%2Fy%22%2C+%22lte%22%3A+%222010%7C%7C%2Fy%22%2C+%22format%22%3A+%22year%22%7D%7D%7D%2C+%7B%22range%22%3A+%7B%22awardDate%22%3A+%7B%22gte%22%3A+%222011%7C%7C%2Fy%22%2C+%22lte%22%3A+%222011%7C%7C%2Fy%22%2C+%22format%22%3A+%22year%22%7D%7D%7D%2C+%7B%22range%22%3A+%7B%22awardDate%22%3A+%7B%22gte%22%3A+%222012%7C%7C%2Fy%22%2C+%22lte%22%3A+%222012%7C%7C%2Fy%22%2C+%22format%22%3A+%22year%22%7D%7D%7D%2C+%7B%22range%22%3A+%7B%22awardDate%22%3A+%7B%22gte%22%3A+%222013%7C%7C%2Fy%22%2C+%22lte%22%3A+%222013%7C%7C%2Fy%22%2C+%22format%22%3A+%22year%22%7D%7D%7D%2C+%7B%22range%22%3A+%7B%22awardDate%22%3A+%7B%22gte%22%3A+%222014%7C%7C%2Fy%22%2C+%22lte%22%3A+%222014%7C%7C%2Fy%22%2C+%22format%22%3A+%22year%22%7D%7D%7D%2C+%7B%22range%22%3A+%7B%22awardDate%22%3A+%7B%22gte%22%3A+%222015%7C%7C%2Fy%22%2C+%22lte%22%3A+%222015%7C%7C%2Fy%22%2C+%22format%22%3A+%22year%22%7D%7D%7D%2C+%7B%22range%22%3A+%7B%22awardDate%22%3A+%7B%22gte%22%3A+%222016%7C%7C%2Fy%22%2C+%22lte%22%3A+%222016%7C%7C%2Fy%22%2C+%22format%22%3A+%22year%22%7D%7D%7D%2C+%7B%22range%22%3A+%7B%22awardDate%22%3A+%7B%22gte%22%3A+%222017%7C%7C%2Fy%22%2C+%22lte%22%3A+%222017%7C%7C%2Fy%22%2C+%22format%22%3A+%22year%22%7D%7D%7D%2C+%7B%22range%22%3A+%7B%22awardDate%22%3A+%7B%22gte%22%3A+%222009%7C%7C%2Fy%22%2C+%22lte%22%3A+%222009%7C%7C%2Fy%22%2C+%22format%22%3A+%22year%22%7D%7D%7D%2C+%7B%22range%22%3A+%7B%22awardDate%22%3A+%7B%22gte%22%3A+%222008%7C%7C%2Fy%22%2C+%22lte%22%3A+%222008%7C%7C%2Fy%22%2C+%22format%22%3A+%22year%22%7D%7D%7D%5D%7D%7D%2C+%7B%22bool%22%3A+%7B%22should%22%3A+%5B%7B%22term%22%3A+%7B%22recipientRegionName%22%3A+%22West+Midlands%22%7D%7D%5D%7D%7D%2C+%7B%22bool%22%3A+%7B%22should%22%3A+%5B%5D%7D%7D%2C+%7B%22bool%22%3A+%7B%22should%22%3A+%5B%5D%7D%7D%5D%7D%7D%2C+%22sort%22%3A+%7B%22_score%22%3A+%7B%22order%22%3A+%22desc%22%7D%7D%2C+%22extra_context%22%3A+%7B%22amountAwardedFixed_facet_size%22%3A+3%2C+%22awardYear_facet_size%22%3A+50%7D%7D&page="
    page = range(0,729)

    dataset = {}
    index = 0

    for i in page:
      full_url = url+str(i)
      web = requests.get(full_url)
      root = html.fromstring(web.content)
      print "url", root

      rows = root.cssselect("div.panel-body.panel-less-padding")
      print "rows", rows
    
      for row in rows:
         index = index+1
         dataset['index'] = index
         Names = row.cssselect("h4")
         dataset["Names"] = Names[0].text_content()
         Dates = row.cssselect("small.pull-right")
         dataset["Dates"] = Dates[0].text_content()
         Description = row.cssselect('div.panel-body p')
         dataset["Description"] = Description[0].text_content()
         Amount = row.cssselect('div.panel-body')
         dataset["Amount"] = Amount[0].text_content().split("Amount:")[1].split("Funder")[0]
         Funder = row.cssselect('div.panel-body a')
         dataset["Funder"] = Funder[1].text_content()
         Recipient = row.cssselect('div.panel-body a')
         dataset["Recipient"] = Recipient[2].text_content()
         District = row.cssselect('div.panel-body a')
         dataset["District"] = District[4].text_content()

         scraperwiki.sql.save(['index'], dataset)

