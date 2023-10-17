# bootcamp_data_engineering
Bootcamp Data Engineering

#Adding Regions into the query

*1. Add into _tpch_sf1__sources.yml*

```python
- name: region
        description: Regions table holding repetitive data.
        columns:
        - name: r_regionkey
          description: This is the Region Key.
          tests:
            - not_null
            - unique
```
![image](https://github.com/rabelogu/bootcamp_data_engineering/assets/45816059/5c341d37-5e79-42bf-b6df-6b815c790cca)

*2. Create the SQL query*
```SQL
   WITH
source AS (
    SELECT
        *
    FROM {{ source('src_tpch_sf1', 'region') }}
)

SELECT
    r_regionkey AS region_key,
    r_name AS region_name
FROM source
```
![image](https://github.com/rabelogu/bootcamp_data_engineering/assets/45816059/5cedae5c-c9ae-4e75-bf8c-2ca71a85a79d)

*3. Alter the DIM_CUSTOMER query*
```python
   {{ config(
    materialized = 'table',
) }}


WITH
customer AS (
    SELECT
        *
    FROM {{ ref('stg_tpch_sf1__customer') }}
),

nation AS (
    SELECT
        *
    FROM {{ ref('stg_tpch_sf1__nation') }}
),

region AS (
    SELECT
        *
    FROM {{ ref('stg_tpch_sf1__region') }}
)

SELECT
    customer.cust_key,
    customer.cust_name,
    customer.cust_address,
    nation.nation_name,
    region.region_name,
    customer.cust_phone
FROM customer
LEFT JOIN nation
    ON (customer.nation_key = nation.nation_key)
LEFT JOIN region
    ON (nation.region_key = region.region_key)
```
![image](https://github.com/rabelogu/bootcamp_data_engineering/assets/45816059/36b6e28c-fa10-4e87-bd72-ec1b20df518a)

#Using PowerBI with Snowflake to data visualization

*1. Add connection*

![PowerBI_SNOWFLAKE_CONNECTION-0](https://github.com/rabelogu/bootcamp_data_engineering/assets/45816059/581a1f8f-8c58-4db2-8e64-60c529eb6203)

![PowerBI_SNOWFLAKE_CONNECTION-1](https://github.com/rabelogu/bootcamp_data_engineering/assets/45816059/7b1de964-2f57-48a7-bedf-6563f11c04e9)

![PowerBI_SNOWFLAKE_CONNECTION-3](https://github.com/rabelogu/bootcamp_data_engineering/assets/45816059/0706fcf5-6150-467b-a49e-a08fcf56199e)

![PowerBI_SNOWFLAKE_CONNECTION-2](https://github.com/rabelogu/bootcamp_data_engineering/assets/45816059/ca9665c9-cb63-4833-be2f-27ef8fd1d6f9)

*2. Transform data and modelate*

![PowerBI_SNOWFLAKE](https://github.com/rabelogu/bootcamp_data_engineering/assets/45816059/6bae9c30-2420-4043-be8e-cbf47f63f49d)

