---
sidebar_label: Tableau
sidebar_position: 205
slug: /en/connect-a-ui/tableau-and-clickhouse
keywords: [clickhouse, tableau, connect, integrate, ui]
description: Tableau can use ClickHouse databases and tables as a data source.
---

# Connecting Tableau to ClickHouse

Tableau can use ClickHouse databases and tables as a data source. This requires a special JDBC driver to be downloaded and saved into a specific location where Tableau can find it.

## 1.  Download the JDBC Driver

The Tableau connector is an extension of the ClickHouse JDBC driver, so you need to download the JDBC driver and save it in the correct folder.

1. Download the latest version of the ClickHouse JDBC driver at <a href="" target="_blank"  >https://github.com/ClickHouse/clickhouse-jdbc/releases/</a>. (We used <a href="https://github.com/ClickHouse/clickhouse-jdbc/releases/download/v0.3.1-patch/clickhouse-jdbc-0.3.1-patch-shaded.jar">this version of the driver</a> for this tutorial.)


:::note
Make sure you download the **clickhouse-jdbc-x.x.x-shaded.jar** JAR file.
:::


2. Store the JDBC driver in the following folder (based on your OS):

    | Operating System  | Destination folder |
    | ----------- | ----------- |
    | MacOS      |  **~/Library/Tableau/Drivers**  |
    | Windows   |  **C:\Program Files\Tableau\Drivers** |

    <br/>

    That's it. The driver will be found the next time you start Tableau.


***

## 3. Download the Connector

ANALYTIKA PLUS has built a handy connector for simplifying connections to ClickHouse from Tableau. You can <a href="https://github.com/analytikaplus/clickhouse-tableau-connector-jdbc" target="_blank"  > view the details of the project in Github</a>. Follow these steps to download the connector...


1. The connector is built in a **taco** file (short for **Ta**bleau **Co**nnector). Download the latest version at <a href="https://github.com/analytikaplus/clickhouse-tableau-connector-jdbc/releases/" target="_blank"  >https://github.com/analytikaplus/clickhouse-tableau-connector-jdbc/releases/</a>. (For this lesson, we downloaded **v0.1.1** of **clickhouse_jdbc.taco**.)

2. Store **clickhouse_jdbc.taco** in the following folder (based on your OS):

    | Operating System  | Destination folder |
    | ----------- | ----------- |
    | MacOS      |  **~/Documents/My Tableau Repository/Connectors**  |
    | Windows   |  **C:\Users\[Windows User]\Documents\My Tableau Repository\Connectors** |

<br/>
The connector is now ready to go.

***

## 4.  Configure a ClickHouse data source in Tableau

Now that you have the driver and connector in the approriate folders on your machine, let's see how to define a data source in Tableau that connects to the **TPCD** database in ClickHouse.


1. Start Tableau. (If you already had it running, then restart it.)

2. From the left-side menu, click on **More** under the **To a Server** section. If everything worked properly, you should see **ClickHouse (JDBC) by ANALYTIKA PLUS** in the list of installed connectors:

    ![ClickHouse (JDBC) by ANALYTIKA PLUS](./images/tableau_connecttoserver.png)

3. Click on **ClickHouse (JDBC) by ANALYTIKA PLUS**  and a dialog window pops up. Enter the following details:

    | Setting  | Value |
    | ----------- | ----------- |
    | Server      |  **localhost**  |
    | Port   |  **8123** |
    | Database |  **default** |
    | Username | **default** |
    | Password | *leave blank* |

<br/>

Your settings should look like:

!["ClickHouse Settings"](./images/tableau_clickhousesettings.png)

:::note
Our ClickHouse database is named **TPCD**, but you must set the **Database** to **default** in the dialog above, then select **TPCD** for the **Schema** in the next step. (This is likely due to a bug in the connector, so this behavior could change, but for now you must use **default** as the database.)
:::

4. Click the **Sign In** button and you should see a new Tableau workbook:

    !["New Workbook"](./images/tableau_newworkbook.png)

5. Select **TPCD** from the **Schema** dropdown and you should see the list of tables in **TPCD**:

    !["Select TPCD for the Schema"](./images/tableau_tpcdschema.png)

You are now ready to build some visualizations in Tableau!

***


## 5. Building Visualizations in Tableau

Now that have a ClickHouse data source configured in Tableau, let's visualize the data...

1. Drag the **CUSTOMER** table onto the workbook. Notice the columns appear, but the data table is empty:

    ![""](./images/tableau_workbook1.png)

2. Click the **Update Now** button and 100 rows from **CUSTOMER** will populate the table.


3. Drag the **ORDERS** table into the workbook, then set **Custkey** as the relationship field between the two tables:

    ![""](./images/tableau_workbook2.png)

4. You now have the **ORDERS** and **LINEITEM** tables associated with each other as your data source, so you can use this relationship to answer questions about the data. Select the **Sheet 1** tab at the bottom of the workbook.

    ![""](./images/tableau_workbook3.png)


5. Suppose you want to know how many specific items were ordered each year. Drag **Orderdate** from **ORDERS** into the **Columns** section (the horizontal field), then drag **Quantity** from **LINEITEM** into the **Rows**. Tableau will generate the following line chart:

    ![""](./images/tableau_workbook4.png)

Not a very exciting line chart, but the dataset was generated by a script and built for testing query performance, so you will notice there is not a lot of variations in the simulated orders of the TCPD data.

6. Suppose you want to know the average order amount (in dollars) by quarter and also by shipping mode (air, mail, ship, truck, etc.):

    - Click the **New Worksheet** tab create a new sheet
    - Drag **OrderDate** from **ORDERS** into **Columns** and change it from **Year** to **Quarter**
    - Drag **Shipmode** from **LINEITEM** into **Rows**

You should see the following:

![""](./images/tableau_workbook5.png)

7. The **Abc** values are just filling in the space until you drag a metric onto the table. Drag **Totalprice** from **ORDERS** onto the table. Notice the default calculation is to **SUM** the **Totalpricess**:

    ![""](./images/tableau_workbook6.png)

8. Click on **SUM** and change the **Measure** to **Average**. From the same dropdown menu, select **Format** change the **Numbers** to **Currency (Standard)**:

    ![""](./images/tableau_workbook7.png)

  Well done! You have successfully connected Tableau to ClickHouse, and you have opened up a whole world of possibilities for analyzing and visualizing your ClickHouse data.

:::note
Tableau is great, and we love that it connects so nicely to ClickHouse! If you are new to Tableau, <a href="https://help.tableau.com/current/pro/desktop/en-us/gettingstarted_overview.htm" target="_blank"  >check out their documentation</a> for help on building dashboards and visualizations.
:::


***


**Summary:** You can connect Tableau to ClickHouse using the generic ODBC/JDBC ClickHouse driver, but we really like how this tool from ANALYTIKA PLUS simplifies the process of setting up the connection. If you have any issues with the connector, feel free to reach out to ANALYTIKA PLUS on <a href="https://github.com/analytikaplus/clickhouse-tableau-connector-jdbc/issues" target="_blank"  >GitHub</a>.
