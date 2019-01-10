Create a Magento admin page:
Reference: https://devdocs.magento.com/guides/v2.2/ext-best-practices/extension-coding/example-module-adminpage.html 
 
Files/Folders created:
1.  MyCompany - "Company Name"
2.  MyCompany/ExampleAdminNewPage - "Module Name"
3.  registration.php - Declared, going to develop "MODULE" i.e. not "THEME"
4.  etc/module.xml - Declared, module name (which is actually "Comany Name"_"Actual Module Name") with it's version going to be released.
5.  etc/adminhtml/routes.xml - defined frontName; all pages of this module, start with this frontName then Controller & in last action name.
6.  etc/adminhtml/menu.xml - Prepare/make-ready/add all pages links of this module in menu i.e. decide here, where the pages links would be appear in admin-menu.
7.  Controller - "Controllers of this module would be listed here".
8.  Controller/Adminhtml - "Controllers whose are proposed to be developed for admin/backend only, would be listed here".
9.  Controller/Adminhtml/HelloWorld - "Actually, directory HelloWorld is a Controller. In Magento, in real, Controllers are folder-name like here, not any php-file".
10. Index.php - "It's an action of Controller HelloWorld. In Magento, inside Controller which is actually just any folder/folder-name, all php files are actions of that Controller".
11. view - "Templates & layouts of this module's pages would exist here".
12. view/adminhtml - "Only admin/backend pages layouts & templates would be listed here".
13. view/adminhtml/layout - "Layout xml files would be listed here".
14. exampleadminnewpage_helloworld_index - "A layout file". Remember, naming convention is as "Module frontName defined in routes.xml"_"Controller Name"_"Action Name". This means, for one Controller(with exactlly one action), there defines exactlly one layout. It contains blocks(from Block folder) mapped with templates(from view/adminhtml/templates folder). 
15. view/adminhtml/templates - "Templates i.e. .phtml files would be listed here".
16. helloworld.phtml - "A template file". "One or many template files collectively builds a page".
