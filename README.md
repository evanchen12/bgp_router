# bgp_router
Evan Chen & Kevin Jen
# Approach --- 
For this project we seperate the router into 4 chunks according to the 4 message types. Update message would add the update to our saved_msg list, add an entry to our routing table and update the ASPath. Then it loop through the router's neighbors, sending to all neighbor if update is from a customer else to all remaining neighbors. The withdraw message uses the same loop to send revocation to the router's neighbors. Before passing on the revocation message the router would add the message to saved_msg list and delete routing table entry if the network, netmask and source all matches. For data messages, the router convert the message's destination ip and the ip of each of the entry from the routing table. It would then add the matching entry(ies) into a list. If the list is empty it would send no route message else it would send via the send_data function that checks if customer is either the source or destintation. Tie breaker will occur if the length of the  list >1. For aggregation we have a function that uses a double loop to compare the entries with one another and aggregate accordingly. Disaggregation uses the reconstruct function which utilize the saved_msg list to reconstruct our routing table. Dump message simply dumps the routing table after doing some aggregation.  
# Challenges --- 
- An problem at the start was figuring out how to run the router simulator. This was fixed by running chmod +x 3700router and chmod +x run beforehand.
- Tied breaker for data messages wasn't working as intended for a while. We had a rough time handling peer to customer and vice versa while dropping the message if customer isn't the source nor destination. Another thing we forgot was using long prefix match.
- We were having trouble with aggregation for a long time. We found out that when converting network from decimal to binary, the binaries had leading zeros.
# Design and features --- 
- The into_ip function that aid aggregation by applying -1 netmasking by converting network ip into binary then back to decimal form. This function can also decrease the netmask index by 1. 
# Testing --- 
For testing we use the test file in the configs folder. For looking at more intricate problems we had to use json.dumps since print had no effects
