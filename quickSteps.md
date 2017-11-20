# Quick Steps

## Create

1. Create a resource group

	```
	az group create \
		--name RG_migara \
		--location 'West Europe'
	```

2. Create VNet and a subnet

	```az network vnet create \
			--name vNet \
			--resource-group RG_migara \
			--address-prefixes 10.0.0.0/16 \
			--subnet-name subnet1 \
			--subnet-prefix 10.0.0.0/24
	```

3. Create a route table
	
	```az network route-table create \
			--name RT_1 \
			--resource-group RG_migara
	```

4. Add routes to the route table

	```az network route-table route create \
			--resource-group RG_migara \
			--route-table-name RT_1 \
			--name route_1 \
			--address-prefix 10.0.0.0/28 \
			--next-hop-type VNETLocal
	```

	```az network route-table route create \
			--resource-group RG_migara \
			--route-table-name RT_1 \
			--name route_default \
			--address-prefix 0.0.0.0/0 \
			--next-hop-type VirtualAppliance \
			--next-hop-ip-address 172.24.1.254
	```

5. Assign the route table to the subnet

	```az network vnet subnet update \
			--vnet-name vNet 
			--name subnet1 \
			--resource-group RG_migara 
			--route-table RT_1
	```


## List Routes

	```az network route-table route list \
			--resource-group RG_migara \
			--route-table-name RT_1 \
			--output table
	```

## Cleanup

	```az group delete --name RG_migara --yes```

