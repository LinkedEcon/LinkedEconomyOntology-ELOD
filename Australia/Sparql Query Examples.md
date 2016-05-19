# Example Queries

### Query results can be retrieved by [this google spreadsheet] (https://docs.google.com/spreadsheets/d/1PSSqbP8rQxUrUrSZrp2V_sT5_y811LtFlGAwpruBhiM/edit?usp=sharing).

### Q1. Seller count and amount per City
```
PREFIX elod: <http://linkedeconomy.org/ontology#>
PREFIX vcard: <http://www.w3.org/2006/vcard/ns#>
SELECT DISTINCT (COUNT(DISTINCT ?seller) AS ?sellerCount)
(SUM(xsd:decimal(?amount)) AS ?totalAmount) ?city ?pCode
FROM <http://linkedeconomy.org/Australia>
WHERE {
?payment elod:hasRelatedContract ?contract .
?contract elod:buyer ?buyer ;
          elod:seller ?seller ;
          pc:actualPrice ?ups . 
?ups gr:hasCurrencyValue ?amount .
?seller vcard:hasAddress ?address .
?address vcard:postal-code ?pCode ;
               vcard:locality ?city .
}
GROUP BY ?city ?pCode
ORDER BY DESC (?totalAmount)
```

### Q2. Contract details
```
PREFIX elod: <http://linkedeconomy.org/ontology#>
PREFIX gr: <http://purl.org/goodrelations/v1#>
PREFIX pc: <http://purl.org/procurement/public-contracts#>
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX dcterms: <http://purl.org/dc/terms/>
SELECT ?relatedContract ?issued ?startDate ?endDate (MIN(?sellerName) AS ?sellerName) ?sellerId ?buyerId ?supervisorName (xsd:decimal(?amount) AS ?amount) ?procurementMethod ?description
FROM <http://linkedeconomy.org/Australia>
WHERE {
?payment elod:hasRelatedContract ?contract .
?contract elod:buyer ?buyer ;
          elod:seller ?seller ;
          dcterms:issued ?issued ;
          pc:estimatedEndDate ?endDate ;
          pc:actualPrice ?ups ;   
          pc:item ?offering ;
          pc:procedureType ?procType  ;
          pc:startDate ?startDate .
?seller gr:name ?sellerName .
OPTIONAL {
    ?seller gr:vatID ?sellerId .
} .
?buyer elod:hasSupervisorOrganization ?supervisor ;
       elod:organizationId ?buyerId .
?supervisor gr:legalName ?supervisorName .
?ups gr:hasCurrencyValue ?amount .
?procType skos:prefLabel ?procurementMethod .
?offering gr:includesObject ?tqn .
?tqn gr:typeOfGood ?someItems .
?someItems gr:description ?description .
FILTER (langMatches(lang(?procurementMethod), "el")) .
}
ORDER BY DESC (?paymentAmount)
```

### Q3. Stronger pairs of buyer and Seller
```
PREFIX elod: <http://linkedeconomy.org/ontology#>
PREFIX gr: <http://purl.org/goodrelations/v1#>
PREFIX pc: <http://purl.org/procurement/public-contracts#>
SELECT DISTINCT ?supervisor  ?seller (SUM(xsd:decimal(?amount)) AS ?totalAmount)
FROM <http://linkedeconomy.org/Australia>
WHERE {
?payment elod:hasRelatedContract ?contract .
?contract elod:buyer ?buyer ;
      	  elod:seller ?seller ;
          pc:actualPrice ?ups .  
?buyer elod:hasSupervisorOrganization ?supervisor .
?ups gr:hasCurrencyValue ?amount .
}
GROUP BY ?seller ?supervisor
ORDER BY DESC(?totalAmount)
```

### Q4. Buyer count per supervisor
```
PREFIX gr: <http://purl.org/goodrelations/v1#>
PREFIX elod: <http://linkedeconomy.org/ontology#>
SELECT ?supervisorName (COUNT(DISTINCT ?buyer) AS ?buyerCount)
FROM <http://linkedeconomy.org/Australia>
WHERE	{
?supervisor elod:isSupervisorOrganizationOf ?buyer ;
            gr:legalName ?supervisorName .
}
GROUP BY ?supervisorName
ORDER BY DESC (?buyerCount)
```

### Q5. Seller count and amount per country
```
PREFIX elod: <http://linkedeconomy.org/ontology#>
PREFIX vcard: <http://www.w3.org/2006/vcard/ns#>
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
SELECT (COUNT(DISTINCT ?seller) AS ?sellerCount)
(SUM(xsd:decimal(?amount)) AS ?totalAmount) ?countryName
FROM <http://linkedeconomy.org/Australia>
WHERE {
?payment elod:hasRelatedContract ?contract .
?contract elod:seller ?seller ;
          elod:buyer ?buyer ;
          pc:actualPrice ?ups .  
?ups gr:hasCurrencyValue ?amount .
?seller vcard:hasAddress ?address .
?address vcard:country-name ?country .
?country skos:prefLabel ?countryName .
FILTER (langMatches(lang(?countryName), "el")) .
}
GROUP BY ?countryName
ORDER BY DESC (?totalAmount)
```

### Q6. Buyer contract count with supervisor name
```
PREFIX elod: <http://linkedeconomy.org/ontology#>
PREFIX pc: <http://purl.org/procurement/public-contracts#>
SELECT ?buyer ?supervisorName (COUNT(?contract) AS ?contractsCount)
FROM <http://linkedeconomy.org/Australia>
WHERE{
?payment elod:hasRelatedContract ?contract .
?contract elod:buyer ?buyer ;
          elod:seller ?seller .
?buyer elod:hasSupervisorOrganization ?supervisor .
?supervisor gr:legalName ?supervisorName .
}
GROUP BY ?buyer ?supervisorName
ORDER BY DESC (?contractsCount)
```

### Q7. Supervisor count
```
PREFIX elod: <http://linkedeconomy.org/ontology#>
SELECT (COUNT(DISTINCT ?supervisor) AS ?supervisorrCount)
FROM <http://linkedeconomy.org/Australia>
WHERE{
?payment elod:hasRelatedContract ?contract .
?contract elod:buyer ?buyer ;
          elod:seller ?seller .
?buyer elod:hasSupervisorOrganization ?supervisor .
}
```

### Q8. Contracts count and their total amount
```
PREFIX elod: <http://linkedeconomy.org/ontology#>
PREFIX gr: <http://purl.org/goodrelations/v1#>
PREFIX pc: <http://purl.org/procurement/public-contracts#>
SELECT (COUNT(DISTINCT ?contract) AS ?contractsCount) (xsd:decimal(SUM(?amount)) AS ?totalContractsAmount)
FROM <http://linkedeconomy.org/Australia>
WHERE	{
?payment elod:hasRelatedContract ?contract .
?contract pc:actualPrice ?ups ;
          elod:buyer ?buyer ;
          elod:seller ?seller .
?ups gr:hasCurrencyValue ?amount .
}
```

### Q9. All countries with at least a seller
```
PREFIX elod: <http://linkedeconomy.org/ontology#>
PREFIX vcard: <http://www.w3.org/2006/vcard/ns#>
PREFIX pc: <http://purl.org/procurement/public-contracts#>
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
SELECT DISTINCT ?countryNameGr ?countryNameEn
FROM <http://linkedeconomy.org/Australia>
WHERE {
?payment elod:hasRelatedContract ?contract .
?contract elod:seller ?seller ;
          elod:buyer ?buyer ;
          pc:actualPrice ?ups .
?ups gr:hasCurrencyValue ?amount .
?seller vcard:hasAddress ?address .
?address vcard:country-name ?country .
?country skos:prefLabel ?countryNameGr .
?country skos:prefLabel ?countryNameEn .
FILTER(langMatches(lang(?countryNameGr), "el")) .
FILTER(langMatches(lang(?countryNameEn), "en")) .
}
ORDER BY DESC (?countryNameGr)
```

### Q10. Payments count and their total amount
```
PREFIX gr: <http://purl.org/goodrelations/v1#>
PREFIX elod: <http://linkedeconomy.org/ontology#>
SELECT (COUNT(DISTINCT ?payment) AS ?paymentsCount) (xsd:decimal(SUM(?amount)) AS ?totalPeymentsAmount)
FROM <http://linkedeconomy.org/Australia>
WHERE	
{
?payment elod:hasExpenditureLine ?expline .
?expline elod:amount ?ups .
?ups gr:hasCurrencyValue ?amount .
}
```

### Q11. Sellers count
```
PREFIX gr: <http://purl.org/goodrelations/v1#>
PREFIX elod: <http://linkedeconomy.org/ontology#>
SELECT (COUNT(DISTINCT ?seller) AS ?sellerCount)
FROM <http://linkedeconomy.org/Australia>
WHERE {
?payment elod:hasRelatedContract ?contract .
?contract elod:seller ?seller ;
          elod:buyer ?buyer .
}
```

### Q12. Contract count, total amount and unique name count per seller
```
PREFIX elod: <http://linkedeconomy.org/ontology#>
PREFIX pc: <http://purl.org/procurement/public-contracts#>
PREFIX gr: <http://purl.org/goodrelations/v1#>
SELECT ?seller (COUNT(DISTINCT ?contract) AS ?numberOfContracts) (xsd:decimal(SUM(?amount)) AS ?totalAmount)
(COUNT(DISTINCT ?sellerName) AS ?numberOfNames)
FROM <http://linkedeconomy.org/Australia>
WHERE {
?payment elod:hasRelatedContract ?contract .
?contract elod:seller ?seller ;
          elod:buyer ?buyer ;
          pc:actualPrice ?ups .
?ups gr:hasCurrencyValue ?amount .
?seller gr:name ?sellerName .
}
GROUP BY ?seller
ORDER BY DESC (?totalAmount)
```

### Q13. Total amount and contracts count per procurement method
```
PREFIX elod: <http://linkedeconomy.org/ontology#>
PREFIX pc: <http://purl.org/procurement/public-contracts#>
PREFIX gr: <http://purl.org/goodrelations/v1#>
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
SELECT ?procurementMethod (xsd:decimal(SUM(?amount)) AS ?totalAmount) (COUNT(DISTINCT ?contract) AS ?numberOfContracts) 
FROM <http://linkedeconomy.org/Australia>
WHERE	
{
?payment elod:hasRelatedContract ?contract .
?contract pc:actualPrice ?ups ;
          elod:buyer ?buyer ;
          elod:seller ?seller ;
          pc:procedureType ?procType .
?ups gr:hasCurrencyValue ?amount .
?procType skos:prefLabel ?procurementMethod .
FILTER(langMatches(lang(?procurementMethod), "el")) .
}
GROUP BY ?procurementMethod
ORDER BY DESC (?totalAmount)
```

### Q14. Total amount supervisor organization paid per procurement method
```
PREFIX elod: <http://linkedeconomy.org/ontology#>
PREFIX gr: <http://purl.org/goodrelations/v1#>
PREFIX pc: <http://purl.org/procurement/public-contracts#>
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
SELECT ?supervisorName (xsd:decimal(SUM(?amount)) AS ? totalAmountPaid) ?procurementMethod
FROM <http://linkedeconomy.org/Australia>
WHERE	{
<http://linkedeconomy.org/resource/Organization/000>  elod:isSupervisorOrganizationOf ?buyer ;
                gr:legalName ?supervisorName .
?payment elod:hasRelatedContract ?contract .
?contract elod:buyer ?buyer ;
          elod:seller ?seller ;
          pc:actualPrice ?ups ;
   pc:procedureType ?procType .
?ups gr:hasCurrencyValue ?amount .
?procType skos:prefLabel ?procurementMethod .
FILTER(langMatches(lang(?procurementMethod), "el")) .
}
GROUP BY ?supervisorName ?procurementMethod
ORDER BY DESC (?totalAmountPaid)
```

### Q15. Total amount seller received per procurement method
```
PREFIX elod: <http://linkedeconomy.org/ontology#>
PREFIX gr: <http://purl.org/goodrelations/v1#>
PREFIX pc: <http://purl.org/procurement/public-contracts#>
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
SELECT ?seller (xsd:decimal(SUM(?amount)) AS ?totalAmountReceived) ?procurementMethod 
FROM <http://linkedeconomy.org/Australia>
WHERE	
{
?payment elod:hasRelatedContract ?contract .
?contract elod:seller ?seller ;
	  elod:buyer ?buyer ;
          pc:procedureType ?procType ; 
          pc:actualPrice  ?ups .
?seller gr:vatID "50169561394"^^xsd:string .
?ups gr:hasCurrencyValue ?amount .
?procType skos:prefLabel ?procurementMethod .
FILTER(langMatches(lang(?procurementMethod ), "el")) .
}
GROUP BY ?seller ?procurementMethod 
ORDER BY DESC (?totalAmountReceived)
```

### Q16. Seller without ABN
```
PREFIX elod: <http://linkedeconomy.org/ontology#>
PREFIX gr: <http://purl.org/goodrelations/v1#>
SELECT DISTINCT ?seller
FROM <http://linkedeconomy.org/Australia>
WHERE {
?payment elod:hasRelatedContract ?contract .
?contract elod:seller ?seller .
FILTER NOT EXISTS {?seller gr:vatID ?sellerId} .
}
```

### Q17. Top buyer of a seller with total amount and contract count
```
PREFIX elod: <http://linkedeconomy.org/ontology#>
PREFIX gr: <http://purl.org/goodrelations/v1#>
PREFIX pc: <http://purl.org/procurement/public-contracts#>
SELECT ?seller ?supervisorName (xsd:decimal(SUM(?amount)) AS ?totalAmount) (COUNT(DISTINCT ?contract) AS ?contractsCount) 
FROM <http://linkedeconomy.org/Australia>
WHERE {
?payment elod:hasRelatedContract ?contract .
?contract elod:seller ?seller ; 
          elod:buyer ?buyer ;
          pc:actualPrice ?ups .
?seller gr:vatID "50169561394"^^xsd:string .
?ups gr:hasCurrencyValue ?amount .
?buyer elod:hasSupervisorOrganization ?supervisor .
?supervisor gr:legalName ?supervisorName .
} 
GROUP BY ?seller ?supervisorName
ORDER BY DESC (?totalAmount)
LIMIT 1
```

### Q18. All sellers of a specific country with their contracts count and total amount
```
PREFIX elod: <http://linkedeconomy.org/ontology#>
PREFIX pc: <http://purl.org/procurement/public-contracts#>
PREFIX gr: <http://purl.org/goodrelations/v1#>
PREFIX vcard: <http://www.w3.org/2006/vcard/ns#>
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
SELECT DISTINCT ?seller (COUNT(DISTINCT ?contract) AS ?contractsCount) (xsd:decimal(SUM(?amount)) AS ?totalAmount)
FROM <http://linkedeconomy.org/Australia>
WHERE {
?payment elod:hasRelatedContract ?contract .
?contract elod:seller ?seller ;
          elod:buyer ?buyer ;
          pc:actualPrice ?ups .
?seller vcard:hasAddress ?address .
?address vcard:country-name ?country .
?country skos:prefLabel ?countryName .
?ups gr:hasCurrencyValue ?amount .
FILTER(CONTAINS(str(?countryName), "Greece")) .
}
GROUP BY ?seller ?countryName
ORDER BY DESC (?totalAmount)
```
