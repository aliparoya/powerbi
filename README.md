# Material Design Icons for Power BI

This repository contains a CSV file with all 7,000+ icons from Google's Material Design Icons library, formatted for easy use in Power BI.

## Contents

- `outlined_icons.csv`: Contains all Material Design outlined icons with their:
  - Icon names (as used in the official Material Design library)
  - SVG path data ready for Power BI usage

## How to Use in Power BI

Copy and paste this Power Query code into Power BI to load all icons:

powerquery
let
// Load CSV file from GitHub repository
Source = Csv.Document(
Web.Contents("https://raw.githubusercontent.com/aliparoya/powerbi/refs/heads/main/outlined_icons.csv"),
[
Delimiter=",",
Columns=2,
Encoding=65001,
QuoteStyle=QuoteStyle.None
]
),
// Convert CSV to table with headers
ConvertedToTable = Table.PromoteHeaders(Source, [PromoteAllScalars=true]),
// Rename columns to standard naming convention
RenamedColumns = Table.RenameColumns(
ConvertedToTable,
{
{"icon_name", "IconName"},
{"elements", "IconURL"}
}
),
// Transform SVG path data into complete SVG data URL
CreatedSVGUrls = Table.TransformColumns(
RenamedColumns,
{
{
"IconURL",
each "data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' "
& "width='48' height='48' "
& "viewBox='0 -960 960 960'>"
& "<path fill='#000000' d='" & & "'/></svg>"
}
}
)
in
CreatedSVGUrls


This will create a table with two columns:
- `IconName`: The name of the icon as used in Material Design
- `IconURL`: A complete SVG data URL that can be used directly in Power BI visuals

## Customization

You can modify the icons by adjusting the SVG parameters in the code:
- Change `width='48' height='48'` for different icon sizes
- Modify `fill='#000000'` for different colors
- Adjust the `viewBox` settings for different scaling

## Source

Icons are from [Google's Material Design Icons](https://fonts.google.com/icons) library, converted to a Power BI-friendly format.

## License

These icons are available under the [Apache License 2.0](https://github.com/google/material-design-icons/blob/master/LICENSE).
