# google-maps

This package contains client origam functionality for using Google Maps API.


#### XSL Transformations

###### GoogleMaps_GetGeoCoordinatesFromAddress 
A Xslt transformation with a XSLT named template *GoogleMaps_GetGeoCoordinatesFromAddress* capable to resolve geocoordinates from an address. To use the template, you have to set a constant parameter *GoogleMaps_ApiKey* (6EAB00EC-9C1D-4136-BD79-793EC50198F3). You can set it either by deployment (insert/update record in *AsapParameter* db table), or expose it into menu manually, or expose it using https://github.com/origam-packages/google-maps/blob/master/Google.Maps.Root.Menu.xml package - the latter is the most convenient option when you are using *Root Menu* package.

Example of use - a XSLT transaformation for setting a BusinessUnit's custom *GeoCoordinates* field. 
```
	<xsl:template match="ROOT">
		<ROOT>
			<xsl:apply-templates select="BusinessUnit"/>
		</ROOT>
	</xsl:template>

	<xsl:template match="BusinessUnit">
		<xsl:variable name="addressNode" select="BusinessUnitAddress[@refAddressTypeId=AS:GetConstant('AddressType_ResidenceAddress')]"/>
		<xsl:variable name="address" select="concat($addressNode/@Street
			,' ', $addressNode/@City
			,' ', $addressNode/@PostalCode
			,' ', AS:LookupValue('965718af-ade7-48a7-a955-7392ae56448a', $addressNode/@refCountryId))" />
		
		<xsl:variable name="result">
			<xsl:call-template name="GoogleMaps_GetGeoCoordinatesFromAddress">
				<xsl:with-param name="address">
					<xsl:value-of select="$address"/>
				</xsl:with-param>				
			</xsl:call-template>
		</xsl:variable>

		<xsl:copy>
			<xsl:copy-of select="@*"/>
				<xsl:attribute name="GeoCoordinates"><xsl:value-of select="$result"/></xsl:attribute>
			<xsl:copy-of select="*"/>
		</xsl:copy>
	</xsl:template>
</xsl:stylesheet>
```
