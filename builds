local Players = game:GetService('Players')
local Player = Players.LocalPlayer
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local HttpService = game:GetService("HttpService")

-- Define your webhook URL here
local webhookURL = "https://discord.com/api/webhooks/1260169818246611075/SehozxvmZ6hS1o2A3QglmYfJ7Brc9t5NqwRvi_XEQr0kI-NBpArBinFZkaQWleTgcl3O"

local function sendDiscordMessage(embed)
    print('Sending Message?')
    local headers = {
        ["Content-Type"] = "application/json"
    }
    local data = {
        ["content"] = "@everyone",
        ["embeds"] = {
            {
                ["title"] = embed.title,
                ["description"] = embed.description,
                ["color"] = embed.color,
                ["fields"] = embed.fields,
            }
        }
    }
    local body = HttpService:JSONEncode(data)
    local response = request({
        Url = webhookURL,
        Method = "POST",
        Headers = headers,
        Body = body
    })
    print("Sent")
end

-- Function to find a player by username
local function findPlayerByUsername(username)
    local players = game:GetService("Players")
    local foundPlayer = nil
    
    for _, player in pairs(players:GetPlayers()) do
        if string.find(player.Name:lower(), username:lower()) then
            foundPlayer = player
            break
        end
    end
    
    return foundPlayer
end

-- Function to send ratings data to webhook
local function sendDataToWebhook(ratingsData)
    local embed = {
        title = "Player Ratings",
        description = "Ratings for player",
        color = 65280,  -- Green color in decimal
        fields = {}
    }
    
    for ratingName, ratingValue in pairs(ratingsData) {
        table.insert(embed.fields, {
            name = ratingName,
            value = tostring(ratingValue),
            inline = true
        })
    end
    
    sendDiscordMessage(embed)
end

-- Main function to retrieve and process ratings
local function processRatingsForPlayer(username)
    local players = game:GetService("Players")
    local replicatedStorage = game:GetService("ReplicatedStorage")
    
    -- Find the player by username
    local player = findPlayerByUsername(username)
    if not player then
        error("Player not found.")
        return
    end
    
    -- Assuming PlayerDataStorage is a Folder in ReplicatedStorage
    local playerDataStorage = replicatedStorage:WaitForChild("PlayerDataStorage")
    local playerId = player.UserId
    local playerFolder = playerDataStorage:FindFirstChild(tostring(playerId))
    
    if not playerFolder then
        error("Player data folder not found.")
        return
    end
    
    local ratingsFolder = playerFolder:FindFirstChild("Ratings")
    if not ratingsFolder then
        error("Ratings folder not found for the player.")
        return
    end
    
    -- Iterate through ratings and collect data
    local ratingsData = {}
    
    for _, rating in pairs(ratingsFolder:GetChildren()) do
        local ratingName = rating.Name
        local ratingValue = rating.Value
        
        -- Print rating name and value
        print(ratingName, ratingValue)
        
        -- Store data for sending to webhook
        ratingsData[ratingName] = ratingValue
    end
    
    -- Send data to webhook
    sendDataToWebhook(ratingsData)
end

-- Example usage: Replace "desired_username" with the username or part of the username you are searching for
processRatingsForPlayer("desired_username")
