+++
title = 'Proper DB Migrations - Part deux: Implementation'
date = 2023-11-22T11:43:12+01:00
draft = false
summary = 'The choices that are not easy to undo'
+++

Database migrations is a constant head-ache for every developer creating applications that handle any meaningfull amount of data. Over time, with functionality changes, the database structure needs to be kept in sync.

SQL databases are by definition rigid and require things to be done in the proper order. There are constraints and conventions, you need to adhere to, and mistakes are costly. Having the code and database out of sync can have consequences ranging from annoying (certain calls fail) over bad (the application refuses to start) to catastrophic (mangled or lost data). Having a way to roll back is paramount to get back into a working state while you figure out what went wrong. Customers tend to take issue with down-time and data-loss.

### The three-stage deployment model

This is not something I've come up with, but it is a good idea. It is not required to use CI/CD for this model, but it is highly recommended.

#### Step one: Add support in the database for the new code model

Usually, this involves creating new fields on existing tables, new tables and so on. Usually, on existing tables, fields will be nullable.

#### Step two: Update the code to use the new database model

This typically involves having the code use the new facilities for new data - and as part of the deployment, it is also common to update existing data with default or calculated values.

#### Step three: Clean-up and finalizing the DB-model

This is where you enforce the constraints on the DB, change the columns to be not-nullable, setting defaults where they are missing.

#### Caveats

This deployment model involves (at least) three discrete deployments when there are significant database changes. Consider having development cycles of a month+. It's not a nice place to be, so this model is much more feasable for CI/CD-invoked projects.

### How to deploy?

I've chosen (for now) to have a controller for the up and down endpoints. Ideally, at least the up part should be part of the deployment pipeline. The tricky part is the down endpoint - if a deployment goes wrong, but the migrations went well - you can risk having a deployment unable to do the rollback - and the previous version doesn't know about the way to roll back the database migration. The problem is not as great if you use the model outlined above, but you still want to be able to recover, gracefully, should disaster strike. In the past, I've usually had a console app or similar, able to do the rollback if the deployed code is in a non-working state.