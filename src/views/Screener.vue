
<template>
  <div>
    <v-row>
      <v-col cols="12" lg="12" xl="8">
        <div>
          <div>
            <v-card flat color="transparent">
              <v-card-text>
                <div class="searchbar">
                  <v-autocomplete
                    class="mx-4 searchbar"
                    flat
                    v-on:change="CompanyOversea"
                    label="Search a company"
                    v-model="search"
                    solo-inverted
                    append-icon="mdi-magnify"
                    clearable
                    :items="companies"
                    item-text="name"
                  ></v-autocomplete>
                </div>

                <v-navigation-drawer
                  v-model="drawer"
                  app
                  permanent
                  clipped
                  width="350px"
                  color="#f8f9fe"
                >
                  <!-- <v-app-bar-nav-icon  >Filters</v-app-bar-nav-icon> -->
                  <div class="filter">
                    <v-btn large color="#000000" dark
                      ><v-icon>mdi-filter</v-icon>Filters</v-btn
                    >
                  </div>
                  <v-layout column>
                    <v-list>
                      <!-- <v-list-title class="text-h5 grey lighten-2">
                        Filters
                      </v-list-title> -->

                      <v-card-text>
                        <p class="filter-font">Stock Price</p>
                        <v-row>
                          <v-col class="px-4">
                            <v-range-slider
                              v-model="rangePrice"
                              :step="0.01"
                              :min="0"
                              :max="0.2"
                              hide-details
                              v-on:change="stockpriceRe"
                              class="align-center"
                            >
                              <template v-slot:prepend>
                                <v-text-field
                                  :value="rangePrice[0]"
                                  class="mt-0 pt-0"
                                  hide-details
                                  single-line
                                  type="number"
                                  style="width: 60px"
                                  @change="$set(range, 0, $event)"
                                ></v-text-field>
                              </template>
                              <template v-slot:append>
                                <v-text-field
                                  :value="rangePrice[1]"
                                  class="mt-0 pt-0"
                                  hide-details
                                  single-line
                                  type="number"
                                  style="width: 60px"
                                  @change="$set(range, 1, $event)"
                                ></v-text-field>
                              </template>
                            </v-range-slider>
                          </v-col>
                        </v-row>

                        <p class="filter-font">
                          Market Capitalization in Millions count
                        </p>

                        <v-row>
                          <v-col class="px-4">
                            <v-range-slider
                              v-model="rangeCap"
                              :maxCap="200"
                              :minCap="0"
                              :step="10"
                              hide-details
                              v-on:change="marketcapRe"
                              class="align-center"
                            >
                              <template v-slot:prepend>
                                <v-text-field
                                  :value="rangeCap[0]"
                                  class="mt-0 pt-0"
                                  hide-details
                                  single-line
                                  type="number"
                                  style="width: 60px"
                                  @change="$set(range, 0, $event)"
                                ></v-text-field>
                              </template>
                              <template v-slot:append>
                                <v-text-field
                                  :value="rangeCap[1]"
                                  class="mt-0 pt-0"
                                  hide-details
                                  single-line
                                  type="number"
                                  style="width: 60px"
                                  @change="$set(range, 1, $event)"
                                ></v-text-field>
                              </template>
                            </v-range-slider>
                          </v-col>
                        </v-row>
                        <v-row>
                          <v-col>
                            <v-select
                              v-model="peratio"
                              :items="peratios"
                              v-on:change="peratioRe"
                              label="Price per earning ratio"
                              dense
                            ></v-select>
                          </v-col>
                        </v-row>
                        <v-row>
                          <v-col>
                            <v-select
                              v-model="employee"
                              :items="employees"
                              v-on:change="employeeRe"
                              label="Number of employees"
                              dense
                            ></v-select>
                          </v-col>
                        </v-row>
                        <v-row>
                          <v-col>
                            <v-select
                              v-model="sector"
                              :items="sectors"
                              item-text="sector"
                              v-on:change="sectorRe"
                              label="Sector relations"
                              dense
                            ></v-select>
                          </v-col>
                        </v-row>

                        <v-row>
                          <v-col>
                            <v-select
                              v-model="industry"
                              :items="industries"
                              item-text="industry"
                              v-on:change="industryRe"
                              label="Industry relations"
                              dense
                            ></v-select>
                          </v-col>
                        </v-row>

                        <v-row>
                          <v-col>
                            <v-select
                              v-model="director"
                              :items="directors"
                              item-text="director"
                              v-on:change="directorRe"
                              label="Director relations"
                              dense
                            ></v-select>
                          </v-col>
                        </v-row>

                        <v-row>
                          <v-col>
                            <v-select
                              v-model="place"
                              :items="places"
                              item-text="state"
                              v-on:change="placeRe"
                              label="Principal place of business"
                              dense
                            ></v-select>
                          </v-col>
                        </v-row>

                        <v-row>
                          <v-col>
                            <v-select
                              v-model="compRe"
                              v-on:change="comAllRelation"
                              :items="companies"
                              item-text="name"
                              :menu-props="{ maxHeight: '400' }"
                              persistent-hint
                              label="Company relations"
                              multiple
                              chips
                              dense
                            ></v-select>
                            <!-- <v-checkbox
                              v-model="compRebox"
                              label="Only show related relationships"
                              color="#0000FF"
                              @change="check(comOnlyRelation)"
                              hide-details
                            ></v-checkbox> -->
                          </v-col>
                        </v-row>
                      </v-card-text>
                    </v-list>
                  </v-layout>
                </v-navigation-drawer>

                <v-col class="text-right">
                  <v-btn dark class="text-capitalize" @click="downloadGraph">
                    <v-icon>mdi-download</v-icon>
                    Download graph as image
                  </v-btn>
                </v-col>

                <v-divider class="my-4"></v-divider>

                <script
                  type="application/javascript"
                  src="https://cdn.neo4jlabs.com/neovis.js/v1.5.0/neovis.js"
                ></script>
                <div id="viz" ref="printcontent">
                  <v-row>
                    <v-col cols="12">
                      <div>
                        <v-alert
                          class="font-italic text-center justify-left"
                          border="left"
                          colored-border
                          color="accent"
                        >
                          <h3>
                            Please entry to view the resulted stock graphs!
                          </h3>
                        </v-alert>
                      </div>
                    </v-col>
                  </v-row>
                </div>
              </v-card-text>
            </v-card>
          </div>
        </div>
      </v-col>
    </v-row>
  </div>
</template>
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<script src="http://code.jquery.com/jquery-1.5.2.min.js"></script>

<script >
import neo4j from "neo4j-driver";
import html2canvas from "html2canvas";
export default {
  data: () => ({
    search: "",
    place: "",
    selected: true,
    companies: [],
    sectors: [],
    industries: [],
    directors: [],
    places: [],
    drawer: true,
    rangePrice: [-20, 70],
    price1: "",
    price2: "",
    rangeCap: [-20, 70],
    compRe: [],
    comp1: "",
    comp2: "",
    sector: "",
    industry: "",
    peratio: "",
    employee: "",
    director: "",
    peratios: ["Below 50 Million", "Above 50 Million"],
    employees: ["Below 500", "Above 500"],
    viz: {},
  }),
  mounted() {
    this.companylist();
    this.sectorlist();
    this.industrylist();
    this.directorlist();
    this.placelist();
  },
  methods: {
    async companylist() {
      var companyArr = [];
      var driver = neo4j.driver(
        "neo4j://46243086.databases.neo4j.io:7687",
        neo4j.auth.basic("neo4j", "XIx343iBGlvTvANKgc32XuwQn-n6M1bH-OjhyGV7mlc")
      );
      var session = driver.session();
      //var name = this.name.toLowerCase();

      session
        .run("MATCH (n:Company) RETURN n")
        .then(function (result) {
          result.records.forEach(function (record) {
            companyArr.push({
              name: record._fields[0].properties.name,
              ticker: record._fields[0].properties.ticker,
            });
            session.close();
          });
        })

        .catch(function (err) {
          console.log(err, "hhbj");
        });
      this.companies = companyArr;
      console.log(this.company);
    },
    async sectorlist() {
      var companyArr = [];
      var driver = neo4j.driver(
        "neo4j://46243086.databases.neo4j.io:7687",
        neo4j.auth.basic("neo4j", "XIx343iBGlvTvANKgc32XuwQn-n6M1bH-OjhyGV7mlc")
      );
      var session = driver.session();
      //var name = this.name.toLowerCase();

      session
        .run("MATCH (n:SectorType) RETURN n")
        .then(function (result) {
          result.records.forEach(function (record) {
            companyArr.push({
              sector: record._fields[0].properties.sectorType,
            });
            session.close();
          });
        })

        .catch(function (err) {
          console.log(err, "hhbj");
        });
      console.log(companyArr);
      this.sectors = companyArr;
    },
    async industrylist() {
      var companyArr = [];
      var driver = neo4j.driver(
        "neo4j://46243086.databases.neo4j.io:7687",
        neo4j.auth.basic("neo4j", "XIx343iBGlvTvANKgc32XuwQn-n6M1bH-OjhyGV7mlc")
      );
      var session = driver.session();
      //var name = this.name.toLowerCase();

      session
        .run("MATCH (n:IndustryType) RETURN n")
        .then(function (result) {
          result.records.forEach(function (record) {
            companyArr.push({
              industry: record._fields[0].properties.industryType,
            });
            session.close();
          });
        })

        .catch(function (err) {
          console.log(err, "hhbj");
        });
      this.industries = companyArr;
    },
    async directorlist() {
      var companyArr = [];
      var driver = neo4j.driver(
        "neo4j://46243086.databases.neo4j.io:7687",
        neo4j.auth.basic("neo4j", "XIx343iBGlvTvANKgc32XuwQn-n6M1bH-OjhyGV7mlc")
      );
      var session = driver.session();
      //var name = this.name.toLowerCase();

      session
        .run("MATCH (n:DirectorName) RETURN n")
        .then(function (result) {
          result.records.forEach(function (record) {
            companyArr.push({
              director: record._fields[0].properties.directorName,
            });
            session.close();
          });
        })

        .catch(function (err) {
          console.log(err, "hhbj");
        });
      this.directors = companyArr;
    },
    async placelist() {
      var companyArr = [];
      var driver = neo4j.driver(
        "neo4j://46243086.databases.neo4j.io:7687",
        neo4j.auth.basic("neo4j", "XIx343iBGlvTvANKgc32XuwQn-n6M1bH-OjhyGV7mlc")
      );
      var session = driver.session();
      //var name = this.name.toLowerCase();

      session
        .run("MATCH (n:Location) RETURN n")
        .then(function (result) {
          result.records.forEach(function (record) {
            companyArr.push({
              state: record._fields[0].properties.state,
              city: record._fields[0].properties.city,
            });
            session.close();
          });
        })

        .catch(function (err) {
          console.log(err, "hhbj");
        });
      this.places = companyArr;
      console.log(this.company);
    },

    CompanyOversea() {
      console.log(this.search);
      var search = this.search;
      var config = {
        encrypted: "ENCRYPTION_ON",
        search: this.search,
        container_id: "viz",
        server_url: "neo4j://46243086.databases.neo4j.io:7687",
        server_user: "neo4j",
        server_password: "XIx343iBGlvTvANKgc32XuwQn-n6M1bH-OjhyGV7mlc",
        labels: {
          Company: {
            caption: "ticker",
          },
          DirectorName: {
            caption: "directorName",
          },
          SectorType: {
            caption: "sectorType",
          },
          IndustryType: {
            caption: "industryType",
          },
          MarketValue: {
            caption: "marketValue",
          },
          StockValue: {
            caption: "stockValue",
          },
          PeratioValue: {
            caption: "peratioValue",
          },
          EmployeeCount: {
            caption: "empCount",
          },
          Location: {
            caption: "state",
          },
        },
        relationships: {
          DIRECTED_BY: {
            thickness: "count",
          },
          INDUSTRALIZE_IN: {
            thickness: "count",
          },
          STOCKPRICE_OF: {
            thickness: "count",
          },
          SECTORS_IN: {
            thickness: "count",
          },
          MARKETCAP_IS: {
            thickness: "count",
          },
          HAS_EMPLOYEE: {
            thickness: "count",
          },
          PERATIO_OF: {
            thickness: "count",
          },
          LOCATED_AT: {
            thickness: "count",
          },
        },
        //initial_cypher: "MATCH (c)-[r]->(d) RETURN c,r,d"
        initial_cypher:
          "MATCH p=(Company {name: '" +
          search +
          "'})-[r:DIRECTED_BY|SECTORS_IN|INDUSTRALIZE_IN|MARKETCAP_IS|STOCKPRICE_OF|HAS_EMPLOYEE|PERATIO_OF|LOCATED_AT]->() RETURN p ",
      };

      var viz = new NeoVis.default(config);
      viz.render();
    },
    stockpriceRe() {
      this.price1 = this.rangePrice[0];
      this.price2 = this.rangePrice[1];
      var config = {
        encrypted: "ENCRYPTION_ON",
        container_id: "viz",
        server_url: "neo4j://46243086.databases.neo4j.io:7687",
        server_user: "neo4j",
        server_password: "XIx343iBGlvTvANKgc32XuwQn-n6M1bH-OjhyGV7mlc",
        labels: {
          Company: {
            caption: "ticker",
          },
          StockValue: {
            caption: "stockValue",
          },
        },
        relationships: {
          STOCKPRICE_OF: {
            thickness: "count",
          },
        },
        //initial_cypher: "MATCH (c)-[r]->(d) RETURN c,r,d"
        initial_cypher:
          "MATCH (a:Company)-[r:STOCKPRICE_OF]->(b:StockValue) WHERE b.stockValue >= " +
          this.price1 +
          " AND b.stockValue <= " +
          this.price2 +
          " RETURN a,r,b",
      };
      var viz = new NeoVis.default(config);
      console.log(viz);
      viz.render();
    },
    marketcapRe() {
      this.market1 = this.rangeCap[0];
      this.market2 = this.rangeCap[1];
      var config = {
        encrypted: "ENCRYPTION_ON",
        container_id: "viz",
        server_url: "neo4j://46243086.databases.neo4j.io:7687",
        server_user: "neo4j",
        server_password: "XIx343iBGlvTvANKgc32XuwQn-n6M1bH-OjhyGV7mlc",
        labels: {
          Company: {
            caption: "ticker",
          },

          MarketValue: {
            caption: "marketValue",
          },
        },
        relationships: {
          MARKETCAP_IS: {
            thickness: "count",
          },
        },
        //initial_cypher: "MATCH (c)-[r]->(d) RETURN c,r,d"
        initial_cypher:
          "MATCH (a:Company)-[r:MARKETCAP_IS]->(b:MarketValue) WHERE b.marketValue >= " +
          this.market1 +
          " AND b.marketValue <= " +
          this.market2 +
          " RETURN a,r,b",
      };
      var viz = new NeoVis.default(config);
      console.log(viz);
      viz.render();
    },
    directorRe() {
      var director = this.director;
      var config = {
        encrypted: "ENCRYPTION_ON",
        container_id: "viz",
        server_url: "neo4j://46243086.databases.neo4j.io:7687",
        server_user: "neo4j",
        server_password: "XIx343iBGlvTvANKgc32XuwQn-n6M1bH-OjhyGV7mlc",
        labels: {
          Company: {
            caption: "ticker",
          },
          DirectorName: {
            caption: "directorName",
          },
        },
        relationships: {
          DIRECTED_BY: {
            thickness: "count",
          },
        },
        //initial_cypher: "MATCH (c)-[r]->(d) RETURN c,r,d"
        initial_cypher:
          "MATCH p=(Company)-[r:DIRECTED_BY]->(DirectorName {directorName: '" +
          director +
          "'}) RETURN p ",
      };
      var viz = new NeoVis.default(config);
      viz.render();
    },
    sectorRe() {
      var sector = this.sector;
      var config = {
        encrypted: "ENCRYPTION_ON",
        container_id: "viz",
        server_url: "neo4j://46243086.databases.neo4j.io:7687",
        server_user: "neo4j",
        server_password: "XIx343iBGlvTvANKgc32XuwQn-n6M1bH-OjhyGV7mlc",
        labels: {
          Company: {
            caption: "ticker",
          },
          SectorType: {
            caption: "sectorType",
          },
        },
        relationships: {
          SECTORS_IN: {
            thickness: "count",
          },
        },
        //initial_cypher: "MATCH (c)-[r]->(d) RETURN c,r,d"
        initial_cypher:
          "MATCH p=(Company)-[r:SECTORS_IN]->(SectorType {sectorType: '" +
          sector +
          "'}) RETURN p  ",
      };

      var viz = new NeoVis.default(config);
      viz.render();
    },
    industryRe() {
      var industry = this.industry;
      var config = {
        encrypted: "ENCRYPTION_ON",
        container_id: "viz",
        server_url: "neo4j://46243086.databases.neo4j.io:7687",
        server_user: "neo4j",
        server_password: "XIx343iBGlvTvANKgc32XuwQn-n6M1bH-OjhyGV7mlc",
        labels: {
          Company: {
            caption: "ticker",
          },
          IndustryType: {
            caption: "industryType",
          },
        },
        relationships: {
          INDUSTRALIZE_IN: {
            thickness: "count",
          },
        },
        //initial_cypher: "MATCH (c)-[r]->(d) RETURN c,r,d"
        initial_cypher:
          "MATCH p=(Company)-[r:INDUSTRALIZE_IN]->(IndustryType {industryType: '" +
          industry +
          "'}) RETURN p  ",
      };

      var viz = new NeoVis.default(config);
      console.log(this.industry);
      viz.render();
    },
    placeRe() {
      var place = this.place;
      var config = {
        encrypted: "ENCRYPTION_ON",
        container_id: "viz",
        server_url: "neo4j://46243086.databases.neo4j.io:7687",
        server_user: "neo4j",
        server_password: "XIx343iBGlvTvANKgc32XuwQn-n6M1bH-OjhyGV7mlc",
        labels: {
          Company: {
            caption: "ticker",
          },
          Location: {
            caption: "state",
          },
        },
        relationships: {
          LOCATED_AT: {
            thickness: "count",
          },
        },
        //initial_cypher: "MATCH (c)-[r]->(d) RETURN c,r,d"
        initial_cypher:
          "MATCH p=(Company)-[r:LOCATED_AT]->(Location {state: '" +
          place +
          "'}) RETURN p  ",
      };

      var viz = new NeoVis.default(config);
      console.log(this.industry);
      viz.render();
    },
    peratioRe() {
      if (this.peratio == "Below 50 Million") {
        console.log(this.peratio);
        return this.peratioLessRee();
      }
      if (this.peratio == "Above 50 Million") {
        console.log(this.peratio);
        return this.peratioMoreRee();
      }
    },
    peratioMoreRee() {
      var config = {
        encrypted: "ENCRYPTION_ON",
        container_id: "viz",
        server_url: "neo4j://46243086.databases.neo4j.io:7687",
        server_user: "neo4j",
        server_password: "XIx343iBGlvTvANKgc32XuwQn-n6M1bH-OjhyGV7mlc",
        labels: {
          Company: {
            caption: "ticker",
          },
          PeratioValue: {
            caption: "peratioValue",
          },
        },
        relationships: {
          PERATIO_OF: {
            thickness: "count",
          },
        },
        //initial_cypher: "MATCH (c)-[r]->(d) RETURN c,r,d"
        initial_cypher:
          "MATCH (a:Company)-[r:PERATIO_OF]->(b:PeratioValue) WHERE b.peratioValue >= 50 RETURN a,r,b  ",
      };

      var viz = new NeoVis.default(config);
      console.log(viz);
      viz.render();
    },
    peratioLessRee() {
      var config = {
        encrypted: "ENCRYPTION_ON",
        container_id: "viz",
        server_url: "neo4j://46243086.databases.neo4j.io:7687",
        server_user: "neo4j",
        server_password: "XIx343iBGlvTvANKgc32XuwQn-n6M1bH-OjhyGV7mlc",
        labels: {
          Company: {
            caption: "ticker",
          },

          PeratioValue: {
            caption: "peratioValue",
          },
        },
        relationships: {
          PERATIO_OF: {
            thickness: "count",
          },
        },
        //initial_cypher: "MATCH (c)-[r]->(d) RETURN c,r,d"
        initial_cypher:
          "MATCH (a:Company)-[r:PERATIO_OF]->(b:PeratioValue) WHERE b.peratioValue <= 50 RETURN a,r,b  ",
      };

      var viz = new NeoVis.default(config);
      console.log(viz);
      viz.render();
    },

    employeeRe() {
      if (this.employee == "Above 500") {
        console.log(this.employee);
        return this.employeeMoreRee();
      }
      if (this.employee == "Below 500") {
        console.log(this.employee);
        return this.employeeLessRee();
      }
    },
    employeeMoreRee() {
      var config = {
        encrypted: "ENCRYPTION_ON",
        container_id: "viz",
        server_url: "neo4j://46243086.databases.neo4j.io:7687",
        server_user: "neo4j",
        server_password: "XIx343iBGlvTvANKgc32XuwQn-n6M1bH-OjhyGV7mlc",
        labels: {
          Company: {
            caption: "ticker",
          },

          EmployeeCount: {
            caption: "empCount",
          },
        },
        relationships: {
          HAS_EMPLOYEE: {
            thickness: "count",
          },
        },
        //initial_cypher: "MATCH (c)-[r]->(d) RETURN c,r,d"
        initial_cypher:
          "MATCH (a:Company)-[r:HAS_EMPLOYEE]->(b:EmployeeCount) WHERE b.empCount >= 500 RETURN a,r,b  ",
      };

      var viz = new NeoVis.default(config);
      console.log(viz);
      viz.render();
    },
    employeeLessRee() {
      var config = {
        encrypted: "ENCRYPTION_ON",
        container_id: "viz",
        server_url: "neo4j://46243086.databases.neo4j.io:7687",
        server_user: "neo4j",
        server_password: "XIx343iBGlvTvANKgc32XuwQn-n6M1bH-OjhyGV7mlc",
        labels: {
          Company: {
            caption: "ticker",
          },

          EmployeeCount: {
            caption: "empCount",
          },
        },
        relationships: {
          HAS_EMPLOYEE: {
            thickness: "count",
          },
        },
        //initial_cypher: "MATCH (c)-[r]->(d) RETURN c,r,d"
        initial_cypher:
          "MATCH (a:Company)-[r:HAS_EMPLOYEE]->(b:EmployeeCount) WHERE b.empCount <= 500 RETURN a,r,b  ",
      };

      var viz = new NeoVis.default(config);
      console.log(viz);
      viz.render();
    },

    comAllRelation() {
      const compRe = this.compRe.map((element) => {
        return element.toLowerCase();
      });

      var comp1 = compRe[0];
      var comp2 = compRe[1];
      var comp3 = compRe[2];
      var comp4 = compRe[3];
      var comp5 = compRe[4];
      var comp6 = compRe[5];

      var config = {
        encrypted: "ENCRYPTION_ON",
        container_id: "viz",
        server_url: "neo4j://46243086.databases.neo4j.io:7687",
        server_user: "neo4j",
        server_password: "XIx343iBGlvTvANKgc32XuwQn-n6M1bH-OjhyGV7mlc",
        labels: {
          Company: {
            caption: "ticker",
          },
          DirectorName: {
            caption: "directorName",
          },
          SectorType: {
            caption: "sectorType",
          },
          IndustryType: {
            caption: "industryType",
          },
          MarketValue: {
            caption: "marketValue",
          },
          StockValue: {
            caption: "stockValue",
          },
          PeratioValue: {
            caption: "peratioValue",
          },
          EmployeeCount: {
            caption: "empCount",
          },
          Location: {
            caption: "state",
          },
        },
        relationships: {
          DIRECTED_BY: {
            thickness: "count",
          },
          INDUSTRALIZE_IN: {
            thickness: "count",
          },
          STOCKPRICE_OF: {
            thickness: "count",
          },
          SECTORS_IN: {
            thickness: "count",
          },
          MARKETCAP_IS: {
            thickness: "count",
          },
          HAS_EMPLOYEE: {
            thickness: "count",
          },
          PERATIO_OF: {
            thickness: "count",
          },
          LOCATED_AT: {
            thickness: "count",
          },
        },
        //initial_cypher: "MATCH (c)-[r]->(d) RETURN c,r,d"
        initial_cypher:
          "MATCH (a:Company)-[r]->(b) WHERE a.name IN ['" +
          comp1 +
          "','" +
          comp2 +
          "','" +
          comp3 +
          "','" +
          comp4 +
          "','" +
          comp5 +
          "','" +
          comp6 +
          "'] RETURN a,r,b",
      };

      var viz = new NeoVis.default(config);
      console.log(viz);
      viz.render();
    },
    relation() {
      if ((this.compRebox = true)) {
        return this.comOnlyRelation();
      } else if ((this.compRebox = false)) {
        return this.comAllRelation();
      } else {
        console.log("no purpose");
      }
    },

    downloadGraph() {
      console.log("printing..");

      let div = document.getElementById("viz");

      const link = document.createElement("a");
      link.setAttribute("download", "download.png");
      html2canvas(div).then(function (canvas) {
        link.setAttribute(
          "href",
          canvas
            .toDataURL("image/png")
            .replace("image/png", "image/octet-stream")
        );
        link.click();

        console.log("done");
      });
    },
  },
};
</script>
<style scoped>
#viz {
  width: 920px;
  height: 900px;
  margin-left: 200px;
  text-transform: lowercase;
  border: 2px solid #d3d3d3;
}

.filter-font {
  font-family: sans-serif;
  font-size: 15px;
  color: gray;
}

.searchbar {
  margin-left: 200px;
}

.filter {
  background-color: black;
  height: 45px;
}
</style>
