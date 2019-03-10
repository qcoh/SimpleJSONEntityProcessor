# SimpleJSONEntityProcessor

Process simple JSON responses in Solr's DataImportHandler.

## Build

    mvn package

## Example

I have a simple JSON API at `http://localhost:4000/foo`, which I want to import in Solr using a URLDataSource with SimpleJSONEntityProcessor:

Put the resulting jar file somewhere accessible from Solr. In my case, I added it to `$SOLR/contrib/dataimporthandler-extras/lib`.
Load the jar file by configuring solrconfig.xml. In my case, I added:

    <lib dir="${solr.install.dir:../../../..}/dist/" regex=".*\.jar" />
    <lib dir="${solr.install.dir:../../../..}/contrib/dataimporthandler-extras/lib/" regex=".*\.jar" />

Configure Solr to use DIH in solrconfig.xml:

    <requestHandler name="/dataimport" class="solr.DataImportHandler">
    <lst name="defaults">
      <str name="config">data-config.xml</str>
    </lst>
    </requestHandler>

Configure the DIH (in data-config.xml):

    <dataConfig>
    	<dataSource type="URLDataSource" />
    	<document>
    		<entity name="doc"
    			url="http://localhost:4000/foo"
   			processor="SimpleJSONEntityProcessor">
    			<field column="id" name="id" />
    			<field column="name" name="name_t" />
    		</entity>
    	</document>
    </dataConfig>
