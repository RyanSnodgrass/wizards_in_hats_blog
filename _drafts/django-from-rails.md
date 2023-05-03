# Differences between Rails and Django

## Naming
Rails follows Model View Controller. Model being the database ORM, controller being the middle layer between requests and the server. And Views being the html templates. But in Django they follow Model Template Views. Model == Model. Template == Views. Views == Controller.

## File Structure
Django goes about things the opposite way from Rails. Rails organizes files by functionality. All models go in a app/models/
folder, all controllers go in an app/controllers/ folder. But in Django all files are organized by business purpose. If you have a polling functionality you'll have a polls/ folder where all migrations, urls, models, views, and controllers.

## Routing
in rails all routes go in a config/routes.rb file. But in django every project
