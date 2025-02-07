# Power BI Google Material Icons Integration

This repository provides a Power Query (M) solution for integrating Google Material Icons into Power BI reports. The code transforms a CSV file containing Material Icon definitions into SVG data URLs that can be directly used in Power BI visuals.

## Overview

The solution enables you to:
- Load Material Icons from a CSV source
- Transform icon definitions into browser-compatible SVG data URLs
- Use the icons in Power BI reports with consistent 48x48 pixel dimensions
- Maintain a standardized black fill color (#000000)
- Leverage Material Design's viewBox settings

## Power Query Code

```powerquery
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
                    & "<path fill='#000000' d='" & _ & "'/></svg>"
            }
        }
    )
in
    CreatedSVGUrls
```

## How to Use

1. Copy the Power Query code into your Power BI report
2. The query will create a table with two columns:
   - `IconName`: The name of the Material Icon
   - `IconURL`: The complete SVG data URL that can be used in Power BI

## Features

- **Standardized Dimensions**: All icons are set to 48x48 pixels
- **Consistent Styling**: Black fill color (#000000) for all icons
- **Material Design Viewbox**: Uses the standard Material Design viewBox settings (0 -960 960 960)
- **UTF-8 Encoding**: Ensures proper character handling
- **Browser-Compatible**: Generated SVG URLs work across different browsers

## Use Cases in Power BI

- Custom navigation buttons
- Visual headers and tooltips
- KPI indicators
- Interactive buttons in Power BI reports
- Custom visual elements

## Notes

- The icons are sourced from Google's Material Icons library
- All icons are rendered in black but can be modified by changing the fill color in the SVG path
- The viewBox settings ensure proper scaling and positioning of the icons

## Contributing

Feel free to contribute to this repository by submitting issues or pull requests.

## License

This project uses Google Material Icons which are available under the Apache License 2.0.
