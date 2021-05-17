# antiflood-fivem
Basicamente é um script na qual você consegue limitar a quantidade de vezes que determinada função é chamada em 1 segundo, evitando dumps e flood.

Colocar dentro de vrp > modules

-- **Comando:** vRP.antiflood(source,key,limite)

-- **source** **->** Cidadão que acionou o evento/comando

-- **key** 	**->** Origem do flood, uma forma de identificar onde ele floodou, uma dica é colocar o comando q ta sendo usado o antiflood ou o evento onde está o antiflood (Pode ter qualquer nome)

-- **limite** **->** Limite para o antiflood banir o cidadão, você que define.

-- Um comando como o chat por exemplo, é bom colocar um valor alto, por volta de 8 pra evitar banir o pessoal que manda muita mensagem.

-- Em um evento/função que vc sabe q não tem como floodar sem querer, tipo, tirar item do baú por exemplo, você pode colocar uma tolêrancia baixa, tipo 3, pois você sabe que no intervalo de 1 segundo o cara não faz nessa velocidade.


**--Exemplos de uso:**

-- Antiflood no comando ilegal evitando que o povo floode

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


-- Antiflood em um evento de spawn de dinheiro

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


Créditos: Cowboy Leaks
