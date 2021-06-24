# Mapping guide

## Initialising & Uploading
1. Decide on a namespace. **Make sure that it is not taken.** A list of namespaces will be recorded.
2. Create a PLA json file and a node json file for each namespace. **They are separate**
3. Map the city. Steps are covered in another section.
4. Do check with 7d often if you are doing it properly, especially for the first few times.
5. If he approves, commit and push to your own branch in the repo for data storage.
6. If your namespace is complete, create a pull request for 7d to merge :)

## Plotting a single node
You can either type it in manually, or use the node builder.
Json format:
```
{
  "<node name>": {
    "x": 2, // x coord of node
    "y": 5, // y coord of node
    "connections": [] // leave this empty first
  },
  //more here
}
```

## Plotting a PLA
(aka component)
You have to type this in manually; in the future I might do a builder for this
Json format:
```
{
  "<pla name>": {
    "type": "<type>", // PLA type
    "displayname": "<displayed name>", // this will show on the map
    "description": "<desciption>", // description about the PLA
    "layer": 0, // tells it whether to render this PLA over or under everything else. Default is 0
    "nodes": [], // a list of nodes that the PLA connects to
    "attrs": {} // this one can be empty first
  }
  // more here
}
```

## Using the node builder
1. Get Python version 3.8.0 - 3.9.5 (3.10 doesnt work)
2. Run `pip install tile-renderer` in console
3. `cd` to the directory where your node json file is in
4. Run `renderer nodebuilder` or `python -m renderer builder`
5. Create the node file, and write `{}` inside it. **This is important, or else running the nodebuilder will get you an error.**
6. Indicate the file to write to, eg. `XXX-nodes.json`
7. Ingame, do F3+C, and paste it in the input. You are then prompted to title the node.
8. Repeat this until you are done, then type `exit`.
9. The new nodes should now be in the json file.
10. Remember to save often by exiting and re-entering :D

## ID naming guidelines
* **Remember namespace prefix!** Prefix them like this: `<namespace>-<node name>`, eg `enc-cabrilloAv`
* **No spaces in IDs**
* **Roads** for nodes use `<name><nodeNo>`, for pla use `<name>`. Eg `enc-cabrilloAv2`
* **Double-lined roads, eg A/B roads** for nodes use `<name><n|s|e|w><node No>`, for pla use `<name><n|s|e|w>`, eg `B209n1`. Node number should be the same in one direction, ie dont have n1 and s109 next to each other, have n1 and s1
* **Intersections** for nodes use `<name><nodeNo>_<name><nodeNo>`, eg `frn-insanityBlvd2_accidentAv1`
* **Buildings, parks, platforms, etc** for nodes use `<name><nodeNo>`, for pla use `<name>`. if you have similar buildings with no names ID them with A B C D etc. Eg `enc-officeB1` and `enc-officeB`
* **Grouping PLAs** Use `<group name>_<name etc>`, eg. `frn-cityHall_pondA1_sidewalkB8`

## Viewing progress
* Go to https://map.iiiii7d.repl.co/
* Open Inspect Element, and go to the console
* Type `dataViewer()`.
* You will be prompted to paste in your PLA and node file.
* All PLAs and nodes will then be plotted directly on the map.

## Steps to map a city
1a. Map the roads. They are linear components. if there is an intersection, two roads can go onto one road.
- A list of road types: localHighwaySlip, bRoadSlip, aRoadSlip, localPedestrianQuaternaryRoad, localQuaternaryRoad, localPedestrianTertiaryRoad, localTertiaryRoad, localSecondaryRoad, localMainRoad, localHighway, bRoad, aRoad, rail
- If it is one-way, append the type name with ` oneway` (including the space) For example, `localHighway oneway`
- If it is elevated or underground, append `_elevated` or `_underground`. For example, `localHighway_underground`
- Road crossing? Use `pedestrianCrossing` (point component) on a node where there is a crossing
- Dont map sidewalks!
- Highways should be mapped with two components, both oneway
1b. Map the paths in the city. This includes alleyways, park paths, but **not sidewalks**
- The type is `pathway`. `_elevated` or `_underground` can also be appended.
-  If your path is in the park you can map it together with the park
2a. Map the railways going through the town. The tracks are linear as well. 
- The only type is `rail`.
- `_elevated` and `_underground` also can be appended too.
- Rail crossing? Use `railCrossing` (point component)
2b. Map the stations in the town.
- The type for station buildings is `transportBuilding` and for platforms is `platform`. If you have underground structures, append `_underground`.
- The platform and the track components should share nodes, ie they shd be connected.
- Mark stations with `railStation` (its a point component, ie it only requires one node), and `undergroundExit` for underground exits
3a. Map the buildings.
- The type is `building`. There is also an `_underground` prefix. For city halls, use `cityHall`.
- The buildings should not connect to the road.
- This requires a lot of nodes :P
- If you have rooftop gardens, map those as well.
3b. Map the parks.
- The type for the area is `park`. (Not a point component).
- If you have a national park, don't map it yet.
- If you have a plaza, use `plaza`.
4. Mark individual services (optional)
- All are point components. They are parking, bikeRack, shop, restaurant, hotel, arcade, supermarket, clinic, library, placeOfWorship, petrol, cinema, bank, gym, shelter, playground, fountain, taxiStand, pickUpDropOff, attraction
- Either you create a new node for the markers, _or you use the "icon" key in the PLA json (soon tm)_
5. Map the zones of the city.
- Types: residentialArea, industrialArea, commercialArea, officeArea, residentialOfficeArea, schoolArea, healthArea, agricultureArea, militaryArea
- If you have a national park area dont use this
- You can anchor to other nodes for this, if possible.
6. Map heliports, seaplane ports, waterports, airports and bus stops.
- Types: gate, apron, taxiway, runway, helipad. Gate and Helipad are area components btw, not a point. Taxiway and runway are lines
- For buildings use `transportBuilding`
- Mark bus stops and waterports with `ferryStop` or `busStop`
- If it is a local ferry you can also map the route with `ferryLine`
7. Map the natural features around your town. This includes national parks and empty patches
- Types: grass, shrub, forest, stone, sand
- If you have lakes use `waterSmall`, if you are next to a large ocean use `waterLarge`
- If you have a small island use `landSmall`; or if its large (like Kazeshima) use `landLarge`
8. Map the subdivisions and town borders. 
- Types: `subdistrict`, `district`, `town`
- They are area components btw

## Important things to remember
- Always namespace all nodes and PLAs. This can be done by providing a 3-letter prefix followed by a dash. For example, `abc-IdkRoad` instead of just `IdkRoad`.
- If you are mapping, do check with me often if the format is correct. You might be mapping it wrong, who knows?
- You can reference nodes from another namespace.

## FAQ
* **This Github thing is too convoluted for me!** You can just dm it to 7d, he will check and upload it for you :)
