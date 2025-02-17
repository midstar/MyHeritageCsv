# MyHeritage CSV Export

The WEB page [MyHeritage](https://www.myheritage.com) do not support "bulk" 
download of DNA matches. 

The script below can be used to download all DNA matches listed on MyHeritage
as a CSV file.

## Instruction

1. Navigate to [MyHeritage](https://www.myheritage.com) in a Google Chrome Browser
2. Open the DNA Matching page
3. Got to the bottom of the page and select as many results per page as possible (probably 50)
4. Right click the page and select "Inspect"
5. Copy the script below and paste it into the "Console" window and press ENTER
6. A csv file will be downloaded

To get more DNA matchings you need to manually navigate to another page (2, 3, 4, ...) and
repeat the procedure. The resulting CSV files can be merged manually in Excel, in a text
editor or on any free online CSV merge tool.

## Script

```js
let CSV_SEPARATOR = ","; 
let headings = [
  "Name",
  "Age",
  "Relationship",
  "Shared DNA (%)",
  "Shared DNA (cM)",
  "Shared segments",
  "Largest segment (cM)"
]
let csv = [headings.join(CSV_SEPARATOR)];
function text(e,query) {
  let r = e.querySelectorAll(query);
  if (r.length > 0) {
    return r[0].innerText;
  }
  return "";
}
let dnaCards = document.getElementsByClassName("dna_match_card");
for (let dnaCard of dnaCards) {
  let cols = [
    text(dnaCard,'[data-automations="ProfileNameCallout"]'),
    text(dnaCard,'[class=detail_value]'),
    text(dnaCard,'[data-automations="possible_relationships_popup_trigger"]'),
    text(dnaCard,'[data-automations="QualitySharedDnaPer"]'),
    text(dnaCard,'[data-automations="QualitySharedDnaTotal"]'),
    text(dnaCard,'[data-automations="QualitySharedSegmentsTotal"]'),
    text(dnaCard,'[data-automations="QualityLargestSegmentTotal"]')
  ]
  cols = cols.map(function(item,index){
    return item.replaceAll(',','.')
  });
  csv.push(cols.join(CSV_SEPARATOR));
}
const downloadLink = document.createElement('a');
downloadLink.href = `data:text/plain;charset=utf-8, ${encodeURIComponent(csv.join("\n"))}`;
downloadLink.download = 'myheritage.csv';
document.body.appendChild(downloadLink);
downloadLink.click();
```

