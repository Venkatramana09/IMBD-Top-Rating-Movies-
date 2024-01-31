# IMBD-Top-Rating-Movies-Dashbord

WEB Link: https://www.imdb.com/chart/top/

Used M code to convert ratings K & M:

= Table.AddColumn(#"Replaced Value1", "Rating", each 
    
    if Text.EndsWith([Column3], "K") then 
        Number.FromText(Text.Start([Column3], Text.Length([Column3]) - 1)) * 1000 
    else if Text.EndsWith([Column3], "M") then 
        Number.FromText(Text.Start([Column3], Text.Length([Column3]) - 1)) * 1000000 
    else 
        Number.FromText([Column3])
)


M code to get time converted into minutes:

= let
    SourceText = [Column5],  
    ContainsHour = Text.Contains(SourceText, "h"),
    ContainsMinute = Text.Contains(SourceText, "m"),

    SplitText = Text.Split(SourceText, "h"),

    Hours = if ContainsHour then Number.FromText(Text.Trim(SplitText{0})) else 0,
    Minutes = if ContainsMinute then Number.FromText(Text.Trim(Text.Replace(SplitText{if ContainsHour then 1 else 0}, "m", ""))) else 0,

    TotalMinutes = Hours * 60 + Minutes
in
    TotalMinutes
