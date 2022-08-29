## Nomad custom schema for solar cell efficiency table from an excel file  
This is an exmaple on how to digitise a solar cell efficiency table from a review paper and make every solar cell a nomad archive data entry.

The nomad metainfo schema gets defined in a yaml file. 
Instructions and dcoumentation on how to build such a schema can be found in the [nomad-lab documentation pages](https://nomad-lab.eu/prod/v1/staging/docs/archive.html)  

An exemplary nomad solar cell data schema is in this [yaml file](https://github.com/FAIRmat-Experimental/AreaE_use-cases/blob/main/optoelectronics/solar_cells_efficiency_tables/efficiency_tables.archive.yaml). In the schema we inherit base classes developed for solar cell data.
The base classes are inherited under the key `base_sections`. Documentation of these solar cells general sections can be looked in the 
[nomad metainfo explorer](https://nomad-lab.eu/prod/v1/staging/gui/analyze/metainfo/SolarCellLayer).
In the data schema (yaml file), we can define a table parser inheriting the `base_section`: `- nomad.parsing.tabular.TableRow`.
This allow us to populate the nomad data schema from an [excel file](https://github.com/FAIRmat-Experimental/AreaE_use-cases/blob/main/optoelectronics/solar_cells_efficiency_tables/efficiency_tables.archive.xlsx), just by telling from which column we want to populate a the nomad 
quantity that we define in our [yaml schema](https://github.com/FAIRmat-Experimental/AreaE_use-cases/blob/main/optoelectronics/solar_cells_efficiency_tables/efficiency_tables.archive.yaml)

Below we show that in the `sub_section` `publication_reference`, there is a nomad `quantity` called `DOI_number`
which will get populated from the column called *doi* in the excel file.  
  
	
    definitions:
      name: 'Solar cell schema'
      sections:
        SolarCell:
          # Important: must inherit from both, ElnBaseSection and TableRow
          # `base_sections` are used to inherit from definitions already in nomad. These base classes 
          # do some work in the backgroud, like parsing the values, making quantities serachable and copying
          # values to our `Results` section, where all the entries become interoperable.
          base_sections: 
            # - 'nomad.datamodel.data.EntryData'
            - nomad.datamodel.metainfo.eln.ElnBaseSection
            - nomad.parsing.tabular.TableRow
          m_annotations:
            eln: 
              hide: ['name', 'lab_id'] # We want to hide some quantities defined in ElnBaseSection in our forms.
          quantities:
            description:
              type: str
              m_annotations:
                tabular:
                  name: Institutions and Comments
                eln:
                  component: RichTextEditQuantity
          sub_sections:
            publication_reference:
              section:
                base_sections: 
                  - 'nomad.datamodel.metainfo.eln.PublicationReference'
                  - 'nomad.parsing.tabular.TableRow'
                quantities:
                  DOI_number:
                    type: str
                    m_annotations:
                  tabular:
                    name: doi
                  eln:
                    component: StringEditQuantity
	 
    
More examples on how to build this type of custom schema for tabular data can be found under the example uploads in the beta version of nomad.
This is a [video tutorial](https://www.youtube.com/watch?v=s9Ju4-CgPHc&t=710s) on how to build nomad schemas for tabular data. 

## How to test it

 - Download the [yaml schema](https://github.com/FAIRmat-Experimental/AreaE_use-cases/blob/main/optoelectronics/solar_cells_efficiency_tables/efficiency_tables.archive.yaml) and the [excel file](https://github.com/FAIRmat-Experimental/AreaE_use-cases/blob/main/optoelectronics/solar_cells_efficiency_tables/efficiency_tables.archive.xlsx) to be mapped.
 - Create a nomad upload in the current [NOMAD beta release](https://nomad-lab.eu/prod/v1/staging/gui/about/information) and add the two files together. 
 - Now you are ready to explore the entries. 
