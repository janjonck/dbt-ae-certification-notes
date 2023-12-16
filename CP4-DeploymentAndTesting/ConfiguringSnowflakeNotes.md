**Snowflake Data Warehouse Setup Summary:**

**Definitions:**
- **User:** Single credentialed user with login and password.
- **Role:** Group of users with similar permissions to access resources.
- **Database:** Highest level of abstraction for file storage, can have multiple schemas.
- **Warehouse:** Unit of computing power for data processing, charges based on size and usage duration.

**Recommended Setup:**
1. **One Connection:**
   - Account: your-host.your-region
   - Web UI: https://your-host.your-region.snowflakecomputing.com/console
   - Use the same connection for accessing logical databases and compute warehouses.

2. **Two Databases:**
   - **raw:** Contains raw data, strictly permissioned access.
   - **analytics:** Contains tables and views for analysts and reporting, owned by dbt.

3. **Three Warehouses:**
   - **loading:** For tools like Fivetran and Stitch to perform regular data loads.
   - **transforming:** Used by dbt for data transformations.
   - **reporting:** Connected to by BI tools for analytical queries.

4. **Four Roles:**
   - **public:** Default set of user permissions.
   - **loader:** Owns tables in the raw database, connects to the loading warehouse.
   - **transformer:** Has query permissions on raw database tables, owns analytics database tables.
   - **reporter:** Has permissions on the analytics database only.

5. **Five Types of Users:**
   - **Primary account login:** For setting up and administering resources.
   - **Data loader(s):** Assigned to the loader role.
   - **Transformation scheduler(s):** Assigned to the transformer role.
   - **BI tool(s):** Assigned to the reporter role.
   - **Analysts:** Have their own logins, assigned to the transformer role.

**Recap:**
- By-the-minute cost structure and granular permissions.
- Control permissions at the role level, create unique logins for all users.
  
**Setup Overview:**
- 1 Connection
- 2 Databases: raw, analytics
- 3 Warehouses: loading, transforming, reporting
- 4 Roles: public, loader, transformer, reporter
- 5 Users: admin, etl_user, dbt_cloud_user, bi_user, analyst
