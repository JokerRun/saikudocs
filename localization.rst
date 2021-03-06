Localization
============

It is possible to attain a localized version of a Saiku view in the Saiku standalone version.

Follow these steps:

In the admin pane, add following to the datasource: `"DynamicSchemaProcessor=mondrian.i18n.LocalizingDynamicSchemaProcessor;locale=en_US;"`

Go to `WEB-INF/classes/mondrian.properties` and enable the `LocalizingDynamicSchemaProcessor` by adding following line: `"mondrian.rolap.localePropFile=locale.locale"`. This line tells that you will add a folder 'locale' that contains files prefixed with 'locale' containing the translations.

Create the folder 'locale' in `WEB-inf/classes/` and add some files:
`locale_en_US.properties
locale_fr_FR.properties`

Provide translations in your mondrian schema by changing variable names with the ${} notation.

Provide these translations in the properties files of step 3.
Test your set-up
From this point, you can manually check whether your translations work by changing the locale in the datasource definition.

Enable the ChangeLocale plugin by going to the Saiku Settings.js file and adding 'ChangeLocale' to the PLUGINS array:

.. code-block:: xml 

   PLUGINS: [
     "Chart",
     "ChangeLocale"
   ],

Make sure the locales are mapped in the settings.js file:

.. code-block:: xml 

    Settings.MONDRIAN_LOCALES = {
      "English": "en_US",
      "Dutch": "nl_BE",
      "French": "fr_FR"
    };

A new button  will appear which will change the locale in the data source definition and reload the connection with the translations.

The headers will not be translated in cause you use a Mondrian version prior to 4.3.0.1-SPARK. 

Reports with non-Latin characters
---------------------------------
Saiku reporting engine uses `Apache FOP <https://xmlgraphics.apache.org/fop/>` to generate PDF files. This framework comes out of the box with a set of Adobe's `Base-14 Fonts <https://xmlgraphics.apache.org/fop/0.95/fonts.html#Base-14-Fonts>`, those does not include non-latin characters. So, in order to export PDF reports for languages containing those characters (e.g. Chinese, Greeek, Russian or Arabic), the user need to tell FOP where those fonts are stored and which font to use.

In the Saiku installation directory, locate the file: `"saiku/saiku-core/saiku-web/src/main/resources/org/saiku/web/export/fop_config.xml"`
This file, is well documented, containing examples on how to specify a directory containing True Type fonts, add one specific font or let FOP automatically load the operational system's fonts.

To specify a directory containing True Type fonts (bulk load): 

.. code-block:: xml 

  <!-- "bulk" font mapping -->
  <directory recursive="true">FULL_PATH_TO_YOUR_FONTS_DIRECTORY</directory>

To use one specific font: 

.. code-block:: xml 

  <!-- your specific font mapping -->
  <font kerning="yes" embed-url="FULL_PATH_TO_YOUR_FONT_FILE.ttf" embedding-mode="subset">
      <font-triplet name="YOUR_FONT_NAME" style="normal" weight="normal"/>
  </font>

To let FOP automatically load the operational system's fonts (very slow if you have many fonts installed): 

.. code-block:: xml 

  <!-- automatically detect operating system installed fonts -->
  <auto-detect/>

In order to change the PDF report font, you have to change the file: `"saiku/saiku-core/saiku-web/src/main/resources/org/saiku/web/export/xhtml2fo.xsl"`, the report font size and font family are specified on lines 9 and 10, respectively:

.. code-block:: xml 

  <xsl:param name="font-size" select="'YOUR FONT SIZE'"/>
  <xsl:param name="font.symbol" select="'YOUR FONT FAMILY NAME'"/>

**Be careful to keep parameters values inside sigle quotes.** 