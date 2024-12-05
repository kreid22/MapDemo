This is a POC for the 2024 Innovate initiative.  It includes a map created with the free QGIS desktop software embedded in a basic .Net website.  
We have lots of data with location information that could be turned into maps based on whatever criteria would be useful for internal or external clients.

QGIS is a fairly complex program with many features.  There are several good courses on it in Udemy.

Overview of steps to reproduce:

1.downloaded QGIS mapping software

2. select location data from db.  
Using vk45 growers in each Canadian province for an example

SELECT DISTINCT TOP 50 --50 for this sample data
    CASE 
        WHEN business_name IS NULL OR business_name = '' THEN 
            CONCAT(first_name, ' ', last_name)
        ELSE 
            business_name
    END AS NAME,
Address_1, City, State_Code, Country_Code, Zip_Code, 
Phone, Active, g.Vendor_Key, Email, CP_Region, SP_Region
FROM dbo.Grower g
LEFT JOIN vendor v ON v.vendor_key = g.Vendor_Key
WHERE 
    NOT (
        (business_name IS NULL OR (business_name) = '') AND 
        (first_name IS NULL OR (first_name) = '') AND 
        (last_name IS NULL OR (last_name) = '') 
    )
	AND NOT (g.Address_1 IS NULL OR g.Address_1 = '')
AND g.Vendor_Key = 45
AND g.Country_Code = 'CA'
AND g.State_Code = 'NL' --one for each province
*note: there seems to be some bad data in dev 
 -there's a bunch of canadian addresses with a US country code

3. Geocode addresses - 
there are various services for this but most require payment for bulk conversions
I used a free tool for this sample that will do up to 500 at a time https://www.geoapify.com/tools/geocoding-online/ 

4. create map in QGIS with geocoded data by importing csv files with the sql and geocoded data.

5. Install QGIS2Web plugin and export a web map

6. Find the exported map folder and drop it in the appropriate place in your web project
