##RDF Data Example



```
<http://linkedeconomy.org/resource/SpendingItem/CN6915_A14> a elod:SpendingItem ;
	elod:buyer <http://linkedeconomy.org/resource/Organization/09_270_0_1> ;
	elod:hasExpenditureLine <http://linkedeconomy.org/resource/ExpenditudeLine/CN6915_A14> ;
	elod:hasRelatedContract <http://linkedeconomy.org/resource/Contract/CN6915_A14> .

<http://linkedeconomy.org/resource/Organization/09_270_0_1> a foaf:Organization, gr:BusinessEntity, org:Organization,  rov:RegisteredOrganization ;
	elod:hasSupervisorOrganization <http://linkedeconomy.org/resource/Organization/014> ;
	vcard:hasAddress <http://linkedeconomy.org/resource/Address/09_270_0_1> ;
	elod:organizationId "09_270_0_1"^^xsd:string ;
	elod:division "CORPORATE PROCUREMENT"^^xsd:string ;
	elod:branch "PROCUREMENT SERVICES TECHNOLOGY"^^xsd:string .

	<http://linkedeconomy.org/resource/Organization/014> a foaf:Organization, gr:BusinessEntity, org:Organization,  rov:RegisteredOrganization ;
		gr:legalName "AYSTRALIAN TAXIATION OFFICE"^^xsd:string ;
		elod:isSupervisorOrganizationOf <http://linkedeconomy.org/resource/Organization/09_270_0_1> .
		
	<http://linkedeconomy.org/resource/Address/09_270_0_1> a vcard:Address ;
		vcard:postal-code "2608"^^xsd:string ;
		vcard:country-name <http://linkedeconomy.org/resource/AU> .

	<http://linkedeconomy.org/resource/AU> a skos:Concept, elodGeo:Country;
		skos:prefLabel "Αυστραλία"@el, "Australia"@en .

<http://linkedeconomy.org/resource/ExpenditudeLine/CN6915_A14> a elod:ExpenditureLine;
	elod:amount <http://linkedeconomy.org/resource/UnitPriceSpecification/CN6915-A14> ;
	elod:seller <http://linkedeconomy.org/resource/Organization/18002855085> .

	<http://linkedeconomy.org/resource/UnitPriceSpecification/CN6915_A14> a gr:UnitPriceSpecification;
		gr:hasCurrencyValue "3303595604.45"^^xsd:float ;
		gr:valueAddedTaxIncluded "1"^^xsd:string ;
		elod:hasCurrency <http://linkedeconomy.org/resource/Currency/AUD> .

		<http://linkedeconomy.org/resource/Currency/AUD> a skos:Concept, elod:Currency ;
			elod:currencyIsoCode "AUD"^^xsd:string ;
			skos:prefLabel "Δολάριο Αυστραλίας"@el, "Australian dollar"@en .

	<http://linkedeconomy.org/resource/Organization/18002855085> a foaf:Organization, gr:BusinessEntity, org:Organization, rov:RegisteredOrganization ;
		gr:name "HP ENTERPRISE SERVICES AYSTRALIA PTY LTD"^^xsd:string ;
		vcard:hasAddress <http://linkedeconomy.org/resource/Address/18002855085> ;
		gr:vatID "18002855085"^^xsd"string .

		<http://linkedeconomy.org/resource/Address/18002855085> a vcard:Address ;
			vcard:street-address "LEVEL 3, 2 BARRY DRIVE"^^xsd:string ;
			vcard:postal-code "2601"^^xsd:string ;
			vcard:locality "CANBERRA"^^xsd:string ;
			vcard:country-name <http://linkedeconomy.org/resource/AU> .

<http://linkedeconomy.org/resource/Contract/CN6915_A14> a pc:Contract;
	elod:seller <http://linkedeconomy.org/resource/Organization/18002855085> ;
	elod:buyer <http://linkedeconomy.org/resource/Organization/09_270_0_1> ;
	elod:contractId "CN6915-A14"^^xsd:string ;
	pc:actualPrice <http://linkedeconomy.org/resource/UnitPriceSpecification/CN6915_A14> ;
	pc:item <http://linkedeconomy.org/resource/Offering/CN6915_A14> ;
	pc:procedureType <http://purl.org/procurement/public-contracts#Open> ;
	pc:startDate "1999-06-30T00:00:00"^^xsd:date ;
	pc:estimatedEndDate "2012-06-30T00:00:00"^^xsd:date ;
	elod:contact <http://linkedeconomy.org/resource/Person/CN6915_A14> ;
	dcterms:issued  "1999-07-22T00:00:00"^^xsd:date .

	<http://linkedeconomy.org/resource/Offering/CN6915_A14> a gr:Offering ;
		gr:includesObject <http://linkedeconomy.org/resource/TypeAndQuantityNode/CN6915_A14> ;
		elod:seller <http://linkedeconomy.org/resource/Organization/18002855085> .

		<http://linkedeconomy.org/resource/TypeAndQuantityNode/CN6915_A14> a pc:TypeAndQuantityNode ;
			gr:typeOfGood <http://linkedeconomy.org/resource/SomeItems/CN6915_A14> .

			<http://linkedeconomy.org/resource/SomeItems/CN6915_A14> a gr:SomeItems ;
				gr:description "SERVICE DELIVERY UNDER EDS/ATO..."^^xsd:string ;
				elod:hasUNSPSC <http://linkedeconomy.org/resource/UNSPSC/81110000> .

				<http://linkedeconomy.org/resource/UNSPSC/81110000> a elod:UnspscCategory ;
					elod:unspscSubject "COMPUTER SERVICES"^^xsd:string ;
					elod:unspscCode "81110000"^^xsd:string .

	<http://purl.org/procurement/public-contracts#Open> a skos:Concept;
		skos:inScheme pc:ProcedureTypeScheme ;
		skos:topConceptOf pc:ProcedureTypeScheme ;
		skos:prefLabel "Ανοικτός Διαγωνισμός"@el, "Open"@en .

		<http://purl.org/procurement/public-contracts#ProcedureTypeScheme> a skos:ConceptScheme ;	
			skos:hasTopConcept pc:Open ;
			skos:hasTopConcept pc:Restricted ;
			skos:hasTopConcept pc:Prequalified .

	<http://linkedeconomy.org/resource/Person/CN6915_A14> a foaf:Person ;
		foaf:personPhone "02 621 66301"^^xsd:string ;					
		foaf:personName "ANTHONY MARCHESE"^^xsd:string .

```
