# google-maps

This package contains client origam functionality for using Google Maps API.


GoogleMaps_GetGeoCoordinatesFromAddress 
---------------------------------------
Xslt transformation with a XSLT named template capable to resolve geocoordinates from an address.

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
