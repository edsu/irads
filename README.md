# irads

*irads* parses the metadata and images out of the 3,517 Facebook ads that were
reported to have been bought by the [Internet Research Agency]. These ads were
released by the House Intelligence Committee as a set of redacted PDF files.

### The Data:

https://democrats-intelligence.house.gov/social-media-content/social-media-advertisements.htm

### The Context:

https://democrats-intelligence.house.gov/social-media-content/

### The Metadata:

The results of running the metadata extraction are available here in this
repository, or at this URL:

https://raw.githubusercontent.com/edsu/irads/master/site/index.json

There is also a CSV version available:

https://raw.githubusercontent.com/edsu/irads/master/site/index.csv

Each ad is a JSON object in `index.json` and looks something like this:

```json
{
  "id": 374,
  "pdf": "data/2015-06/P(1)0000054.pdf",
  "image": "images/374.png",
  "text": "Join us because we care. Black matters.\n",
  "url": "https://www.facebook.com/BlaCk-Matters-1579673598947501/",
  "impressions": 137,
  "clicks": 0,
  "spend": {
    "amount": "44.87",
    "currency": "RUB"
  },
  "created": "2015-06-10T02:59:53-07:00",
  "ended": "2015-06-15T03:42:51-07:00",
  "targeting": {
    "location": {
      "united_states": [
        "Baltimore (+20 km) Maryland",
        "St. Louis (+20 km) Missouri"
      ]
    },
    "excluded_connections": [
      "Exclude people who like Black Matters"
    ],
    "age": [
      "18 - 65+"
    ],
    "language": [
      "English (UK)",
      "English (US)"
    ],
    "placements": [
      "News Feed on desktop computers",
      "News Feed on mobile devices"
    ]
  }
}
```

In addition a cropped image of the supplied post will be included in the
`site/images` directory, which is linked from the JSON object using the `.image`
property.

## Build

Here are the steps for downloading the original data and generating the
extracted metadata yourself. This could be useful if you want to tweak the
extraction process.

### Install Tesseract

You will need to install the [Tesseract] OCR engine, which should be as easy as:

    brew install tesseract

For Linux, Windows, and more please check out the [install instructions].

### Get the Data

    % git clone https://github.com/edsu/irads.git
    % cd irads/data
    % wget -i urls.txt
    % for f in `ls *.zip`; do unzip $f; done

### Extract the Images and OCR

The PDFs contain multiple pages each with an embedded image. The first page is
typically a page of metadata, and the second is a screencap of a Facebook post
of some kind. `extract.py` walks across all the PDFs, extracts images, and also
text for each and writes them out right next to the PDF files.

    % cd .. 
    % pip install -r requirements.txt
    % ./extract.py

This can take a while, so examine `extract.log` to see what's going on.

[Internet Research Agency]: https://en.wikipedia.org/wiki/Internet_Research_Agency
[install instructions]: https://github.com/tesseract-ocr/tesseract/wiki
[Tesseract]: https://github.com/tesseract-ocr/tesseract
