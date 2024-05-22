I. Write data to the database   
    ! you may not need sw_extends here. 
    why? -TODO 
  1) your structur 
    
    └── Plugins
           └─── pluginsName
                  └─── src
                        ├─── Core/Content
                            ├───Collection.php
                            ├───Definition.php
                            ├───Entity.php
                        ├─── Migration
                             └── Migration1546422281name.php
                        ├─── Resources
                            ├───config
                                ├─── routes.xml: <import resource="../../Storefront/Controller/*Controller.php" type="annotation" />
                                ├───service.xml: 
                            ├───views
                               └── secsion
                                    └── cms-section-block-container.html. this is your form, everything you need. This is given via Post
    
                               └── page
                                  └──yourForn.html.twig: After submitting your form
                                  └── page
                                      └── page:detail.html.twig: your content 
                        ├─── Service
                            ├───WrintingData.php
                        ├─── Storefront/Controller
                            ├───controller.php
                        └─── pluginsName.php
   2) Migration: https://developer.shopware.com/docs/guides/plugins/plugins/plugin-fundamentals/database-migrations.html

   3)  service.xml:
    3.1)You must register the route: 
       <service id=“DiaReportForm\Service\WritingData”>
            <argument type=“service” id=“dia_pat_data_test.repository” /> <!--id= Name from table -->
        </service>
      service id= the path of your WritingData,
      argument id= name from Table.repository
    3.2) you must register your controller
       if you have Error: csrf_token, you need to push argument: <argument type="service" id="security.csrf.token_manager"/>
    3.3) you need to register your Definition
       <service id="DiaReportForm\Core\Content\DiaReportFormDefinition">
            <tag name="shopware.entity.definition" entity="name your table" /> <!--entity= name from Table -->
        </service>
       
  4) Controller
    
    https://developer.shopware.com/docs/v6.4/guides/plugins/plugins/storefront/add-custom-controller.html 
    
    return $this>renderStorefront('@DiaReportForm/storefront/page/example.html.twig'); : example.html.twig beinhalte, was du zurückgeben möchtest
    
  5) Core/ Content
    
    - collection.php EntityCollection: https://developer.shopware.com/docs/guides/plugins/plugins/framework/data-handling/add-custom-complex-data.html#entitycollection
    - Definition.php Entity Class: Entity class
    - Entity.php : creat entity class
    Adding Custom Complex Data: https://developer.shopware.com/docs/guides/plugins/plugins/framework/data-handling/add-custom-complex-data.html#entity-class
    
 6) Writing Data:
      https://developer.shopware.com/docs/guides/plugins/plugins/framework/data-handling/writing-data.html
    
    public function __construct(EntityRepository $diaPatDataTestRepository)
        {
            $this->diaPatDataTestRepository = $diaPatDataTestRepository;
        }
        
        public function writeData(array $data, Context $context): void
        {
            $id = Uuid::randomHex();// `id` Binary(16) NOT NULL,
            $this->diaPatDataTestRepository->create([
                [
                    'id' => $id,
                    'name' => $data['name'], // 'name' = spalte, $data['name']= Bezeichnung Name
                    first_name' => $data['vorname'],// 'first_name' = spalte name, vorname = label Vorname
                ]
            ], $context);
        }
