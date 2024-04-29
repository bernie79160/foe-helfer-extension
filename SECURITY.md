# Security Policy

## Supported Versions

Use this section to tell people about which versions of your project are
currently being supported with security updates.

| Version | Supported          |
| ------- | ------------------ |
| 5.1.x   | :white_check_mark: |
| 5.0.x   | :x:                |
| 4.0.x   | :white_check_mark: |
| < 4.0   | :x:                |

## Reporting a Vulnerability

Use this section to tell people how to report a vulnerability.

Tell them where to go, how often they can expect to get an update on a
reported vulnerability, what to expect if the vulnerability is accepted or
declined, etc.
	text-align: left;
}

.textright {
	text-align: right;
}

span.activity {
    display: inline-block;
    height: 14px;
  3 changes: 3 additions & 0 deletions3  
js/web/_i18n/de.json
@@ -209,6 +209,8 @@
    "Boxes.Castle.VisitAntiqueDealerWarning": "Öffne den Antiquitätenhändler um die Daten zu aktualisieren.",
    "Boxes.Castle.VisitCastleWarning": "Öffne die Burg Übersicht um die Daten zu aktualisieren.",
    "Boxes.Castle.VisitGexWarning": "Öffne die GEX Übersicht um die Daten zu aktualisieren.",
    "Boxes.CityMap.Boosts": "Boosts & Produktion",
    "Boxes.CityMap.Building": "Gebäude",
    "Boxes.CityMap.CavalierPerspecitve": "Kavalierperspektive",
    "Boxes.CityMap.CopyMetaInfos": "Daten kopieren",
    "Boxes.CityMap.Desc1": "Um deine Stadt planen zu können, müssen wir deine Daten zu foe-helper.com schicken. Dort kannst du dich dann austoben. <br>Solltest Du dort noch keinen Account haben werden mit dieser Übermittlung Deine Grunddaten mitgesendet. Auf der Webseite kannst Du dann deinen Account registrieren.",
@@ -221,6 +223,7 @@
    "Boxes.CityMap.OlderThan3Era": " liegen 3 ZA zurück",
    "Boxes.CityMap.OlderThan4Era": " liegen 4+ ZA zurück",
    "Boxes.CityMap.OutpostSubmit": "Zum Stadtplaner schicken",
    "Boxes.CityMap.QIHint": "Alle Werte nehmen an, dass die Gebäude fertiggestellt sind.",
    "Boxes.CityMap.ShowNoStreetBuildings": "Markiere Gebäude, die keine Straße brauchen",
    "Boxes.CityMap.ShowSubmitBox": "Stadtplaner",
    "Boxes.CityMap.Submit": "Abschicken",
  3 changes: 3 additions & 0 deletions3  
js/web/_i18n/en.json
@@ -209,6 +209,8 @@
    "Boxes.Castle.VisitAntiqueDealerWarning": "Open the antique dealer to update the data.",
    "Boxes.Castle.VisitCastleWarning": "Open the castle overview to update the data.",
    "Boxes.Castle.VisitGexWarning": "Open the GE overview to update the data.",
    "Boxes.CityMap.Boosts": "Boosts & Production",
    "Boxes.CityMap.Building": "Building",
    "Boxes.CityMap.CavalierPerspecitve": "Side View",
    "Boxes.CityMap.CopyMetaInfos": "Copy city data",
    "Boxes.CityMap.Desc1": "To plan your city on foe-helper.com, we need to send your data to the website. <br>If you don't have an account there yet, your basic data will be sent along with this transmission. You can then register your account on the website.",
@@ -221,6 +223,7 @@
    "Boxes.CityMap.OlderThan3Era": " are 3 Ages behind",
    "Boxes.CityMap.OlderThan4Era": " are 4+ Ages behind",
    "Boxes.CityMap.OutpostSubmit": "Submit to City-Planner",
    "Boxes.CityMap.QIHint": "All displayed values assume your buildings have finished construction.",
    "Boxes.CityMap.ShowNoStreetBuildings": "Highlight buildings that do not need streets",
    "Boxes.CityMap.ShowSubmitBox": "City planner",
    "Boxes.CityMap.Submit": "Submit",
  40 changes: 39 additions & 1 deletion40  
js/web/citymap/css/citymap.css
@@ -76,10 +76,48 @@
	box-sizing: border-box;
}

.outpost #sidebar, .outpost #BuildingsFilter, .outpost #city-map-menu .btn-group {
.outpost #sidebar, .outpost #BuildingsFilter, .outpost #city-map-menu .btn-group, .outpost #map-filters {
	display: none;
}

.outpost.guild_raids #sidebar {
	display: block;
	width: 400px;
	padding: 3px;
}

.outpost.guild_raids #map-container, .outpost.guild_raids #city-map-menu {
	right: 400px;
}

#city-map-overlayBody .prod::before {
	background: transparent url('../../productions/images/productions.png') -233px 2px no-repeat;
	background-size: auto 100%;
	display: inline-block;
	content: '';
	width: 20px;
	height: 20px;
}

#city-map-overlayBody .prod.population::before { background-position: -142px 3px; }
#city-map-overlayBody .prod.happiness::before { background-position: -122px 3px; }
#city-map-overlayBody .prod.guild_raids_money::before { background-position: -20px 3px; }
#city-map-overlayBody .prod.guild_raids_supplies::before { background-position: -40px 3px; }
#city-map-overlayBody .prod.att_def_boost_attacker::before { background-position: -340px 1px; }
#city-map-overlayBody .prod.att_def_boost_defender::before { background-position: -362px 1px; }
#city-map-overlayBody .prod.guild_raids_action_points_collection::before { background-position: -383px 2px; }

#city-map-overlayBody th.population::before, #city-map-overlayBody th.happiness::before {
	background: transparent url('../../productions/images/productions.png') -233px 2px no-repeat;
	background-size: auto 100%;
	display: inline-block;
	content: '';
	width: 23px;
	height: 23px;
}
#city-map-overlayBody th.population::before { background-position: -162px 3px; }
#city-map-overlayBody th.happiness::before { background-position: -140px 3px; }

#sidebar p {
	margin-bottom: 5px;
}
  150 changes: 144 additions & 6 deletions150  
js/web/citymap/js/citymap.js
@@ -33,6 +33,7 @@ let CityMap = {
	EraOutpostData: null,
	EraOutpostAreas: [],
	QIData: null,
	QIStats: null,
	QIAreas: [],


@@ -147,7 +148,7 @@ let CityMap = {
		$('#city-map-overlayHeader > .title').attr('id', 'map' + CityMap.hashCode(Title));

		if (ActiveMap === "cultural_outpost" || ActiveMap === "era_outpost" || ActiveMap === "guild_raids") {
			oB.addClass('outpost')
			oB.addClass('outpost').addClass(ActiveMap)
		}

		let menu = $('<div />').attr('id', 'city-map-menu');
@@ -208,7 +209,7 @@ let CityMap = {
		});

		// Button for submit Box
		if (CityMap.IsExtern === false) {
		if (CityMap.IsExtern === false && ActiveMap === 'main') {
			menu.append($('<input type="text" id="BuildingsFilter" placeholder="'+ i18n('Boxes.CityMap.FilterBuildings') +'" oninput="CityMap.filterBuildings(this.value)">'));
			menu.append(
				$('<div />').addClass('btn-group')
@@ -227,10 +228,14 @@ let CityMap = {
				);
		}

		oB.append(wrapper)
		$('#citymap-wrapper').append(menu)

		if (ActiveMap === "guild_raids") {
			$("#sidebar").append(CityMap.showQIStats())
			$("#sidebar").append(CityMap.showQIBuildings())
		}

		/* In das Menü "schieben" */
		oB.append(wrapper);
		$('#citymap-wrapper').append(menu);
	},


@@ -341,7 +346,6 @@ let CityMap = {
			$('#grid-outer').append( f );
		}

		// Gebäudenamen via Tooltip
		$('.entity').tooltip({
			container: '#city-map-overlayBody',
			html: true
@@ -351,6 +355,140 @@ let CityMap = {
	},


	showQIStats: () => {
		let buildings = Object.values(CityMap.QIData)
		let population = 0, totalPopulation = 0, euphoria = 0, euphoriaBoost = 0, supplies = 0, money = 0, att_def_boost_attacker = 0, att_def_boost_defender = 0
		for (let b in buildings) {
			let building = CityMap.setQIBuilding(MainParser.CityEntities[buildings[b]['cityentity_id']])
			if (building.type !== "impediment" && building.type !== "street") {
				population += building.population
				euphoria += building.euphoria
				totalPopulation += (building.population > 0 ? building.population : 0)

				if (building.boosts !== null) {
					for (let i in building.boosts) {
						let boost = building.boosts[i]
						if (boost.type === "att_def_boost_attacker")
							att_def_boost_attacker += boost.value 
						if (boost.type === "att_def_boost_defender")
							att_def_boost_defender += boost.value 
					}
				}
				if (building.production !== null) {
					if (building.production.guild_raids_supplies)
						supplies += building.production.guild_raids_supplies
					if (building.production.guild_raids_money)
						money += building.production.guild_raids_money
				}
			}
		}
		let euphoriaFactor = euphoria/totalPopulation
		if (euphoriaFactor < 0.2)
			euphoriaBoost = 0.2
		else if (euphoriaFactor > 0.20 && euphoriaFactor <= 0.60)
			euphoriaBoost = 0.6
		else if (euphoriaFactor > 0.60 && euphoriaFactor <= 0.80)
			euphoriaBoost = 0.8
		else if (euphoriaFactor > 0.80 && euphoriaFactor <= 1.20)
			euphoriaBoost = 1
		else if (euphoriaFactor > 1.20 && euphoriaFactor <= 1.40)
			euphoriaBoost = 1.1
		else if (euphoriaFactor > 1.40 && euphoriaFactor <= 2.0)
			euphoriaBoost = 1.2
		else 
			euphoriaBoost = 1.5

		CityMap.QIStats = {
			population: population,
			totalPopulation: totalPopulation,
			euphoria: euphoria,
			euphoriaBoost: euphoriaBoost,
			money: money*euphoriaBoost,
			supplies: supplies*euphoriaBoost,
			att_def_boost_attacker: att_def_boost_attacker,
			att_def_boost_defender: att_def_boost_defender,
		}

		out = '<div class="text-center" style="padding-bottom: 10px">'
		out += '<p><i>'+i18n('Boxes.CityMap.QIHint')+'</i></p>'
		out += '<span class="prod population">'+CityMap.QIStats.population+'/'+CityMap.QIStats.totalPopulation+'</span> '
		out += '<span class="prod happiness">'+CityMap.QIStats.euphoriaBoost*100+'%</span> <br>'
		out += '<span class="prod guild_raids_money">'+HTML.Format(CityMap.QIStats.money)+'</span> '
		out += '<span class="prod guild_raids_supplies">'+HTML.Format(CityMap.QIStats.supplies)+'</span> <br>'
		out += '<span class="prod att_def_boost_attacker">'+CityMap.QIStats.att_def_boost_attacker+'</span> '
		out += '<span class="prod att_def_boost_defender">'+CityMap.QIStats.att_def_boost_defender+'</span> '
		out += "<div>"
		return out
	},


	showQIBuildings: () => {
		let buildings = Object.values(CityMap.QIData)
		buildings.sort((a, b) => {
			const nameA = a.cityentity_id; 
			const nameB = b.cityentity_id;
			if (nameA < nameB) {
			  return -1
			}
			if (nameA > nameB) {
			  return 1
			}
			return 0
		  })

		let out = '<table class="foe-table">'
		out += '<thead><tr><th>'+i18n('Boxes.CityMap.Building')+'</th><th class="population textright"></th><th class="happiness textright"></th><th>'+i18n('Boxes.CityMap.Boosts')+'</th></tr></thead>'
		out += "<tbody>"
		for (let b in buildings) {
			let building = CityMap.setQIBuilding(MainParser.CityEntities[buildings[b]['cityentity_id']])

			if (building.type !== "impediment" && building.type !== "street") {
				out += "<tr><td>" + building.name + "</td>"
				out += '<td class="textright">' + building.population + "</td>"
				out += '<td class="textright">' + building.euphoria + "</td>"
				out += "<td>"
				if (building.production !== null) {
					out += (building.production.guild_raids_supplies ? '<span class="prod guild_raids_supplies">'+HTML.Format(building.production.guild_raids_supplies*CityMap.QIStats.euphoriaBoost)+'</span> ' : " ")
					out += (building.production.guild_raids_money ? '<span class="prod guild_raids_money">'+HTML.Format(building.production.guild_raids_money*CityMap.QIStats.euphoriaBoost)+'</span> ' : "")
				}
				if (building.boosts !== null) {
					for (let i in building.boosts) {
						let boost = building.boosts[i]
						out += '<span class="prod '+boost.type+'">' + boost.value + '</span> '
					}
				}
				out += "</td></tr>"
			}
		}
		out += "</tbody></table>"
		return out
	},


	setQIBuilding: (data) => {
		let production = data.components?.AllAge?.production?.options
		if (production !== undefined && production.length === 1) // goods and units have multiple production options, rest has one
			production = data.components?.AllAge?.production?.options[0]?.products[0]?.playerResources?.resources
		if (data.type === "main_building")
			production = data.available_products[0].product.resources

		let euphoria = data.components?.AllAge?.staticResources?.resources?.resources?.guild_raids_happiness
		let boosts = data.components?.AllAge?.boosts?.boosts
		let population = data.components?.AllAge?.staticResources?.resources?.resources.guild_raids_population

		let building = {
			name: data.name,
			boosts: boosts || null,
			euphoria: euphoria || 0,
			population: population || 0,
			production: production || null,
			type: data.type
		}

		return building
	},


