# antiflood-fivem
Basicamente é um script na qual você consegue limitar a quantidade de vezes que determinada função é chamada, evitando dumps e flood.

-- Comando: vRP.antiflood(source,key,limite)

-- source -> player que acionou o evento/comando

-- key 	-> origem do flood, uma forma de identificar onde ele floodou, uma dica é colocar o comando q ta sendo usado o antiflood ou o evento onde está o antiflood (Pode ter qualquer nome)

-- limite -> tolerancia para o antiflood banir o cara, isso varia de caso pra caso.

-- um comando como o ilegal por exemplo, é bom colocar um valor alto, por volta de 10 pra evitar banir o pessoal que manda mt mensagem picotada no ilegal.

-- em um evento/função que vc sabe q n tem como floodar, tipo, tirar item do bau por exemplo, isso vc pode colocar uma tolerancia baixa, tipo 2 pois você sabe que no intervalor de 1seg o kr n consegue fazer mais requisições que isso.



--Exemplos de uso:

-- antiflood no comando ilegal evitando que o povo floode

	RegisterCommand('ilegal', function(source, args, rawCommand)
	
		vRP.antiflood(source,"/ilegal",10)
		
		local user_id = vRP.getUserId(source)
		if user_id ~= nil then
			local player = vRP.getUserSource(user_id)
			if not vRP.hasPermission(user_id,"policia.permissao") then
				Citizen.CreateThread(function()
					print("[Anonimo] " ..GetPlayerName(player).. " - " ..user_id .. ':' .. rawCommand:sub(7))					
				end)
				TriggerClientEvent('chatMessage', -1, "[@Anonimo]", {255, 255, 255}, rawCommand:sub(7))			
			end
		end
	end)


--antiflood em um evento de spawn de dinheiro

	RegisterServerEvent("reanimar:pagamento_")
	AddEventHandler("reanimar:pagamento_",function()

		vRP.antiflood(source,"/ilegal",3)

		local user_id = vRP.getUserId(source)
		if user_id then
			pagamento = math.random(50,80)
			vRP.giveMoney(user_id,pagamento)
			TriggerClientEvent("Notify",source,"sucesso","Recebeu <b>$"..pagamento.." dólares</b> de gorjeta do americano.")
		end
	end)
