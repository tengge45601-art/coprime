-- Chạy script này qua Executor của bạn (Ví dụ: Solara)
-- Script này tự động quét và gắn ký tự tích xanh  vào tên của bạn trên bảng xếp hạng thật (CoreGui) ngay khi khởi chạy

local Players = game:GetService("Players")
local CoreGui = game:GetService("CoreGui")
local StarterGui = game:GetService("StarterGui")
local localPlayer = Players.LocalPlayer

local SPECIAL_CHAR = "  " -- Ký tự tích xanh bạn muốn chèn

-- Hàm hiển thị thông báo ở góc màn hình
local function showNotification()
    pcall(function()
        StarterGui:SetCore("SendNotification", {
            Title = "Trạng Thái Script",
            Text = "Đã bật và chèn tích xanh thành công!",
            Duration = 5,
        })
    end)
end

local function injectVerifiedBadge()
    pcall(function()
        -- Quét toàn bộ CoreGui (nơi chứa bảng xếp hạng mặc định của Roblox)
        for _, descendant in ipairs(CoreGui:GetDescendants()) do
            if descendant:IsA("TextLabel") or descendant:IsA("TextButton") then
                local text = descendant.Text
                -- Kiểm tra nếu khung chữ đang hiển thị tên tài khoản hoặc tên hiển thị của bạn
                if (text == localPlayer.Name or text == localPlayer.DisplayName) then
                    if not text:find(SPECIAL_CHAR) then
                        descendant.Text = text .. SPECIAL_CHAR
                    end
                end
            end
        end
    end)
end

-- Kích hoạt ngay lập tức khi vừa bật script để có tích xanh luôn không cần chờ vòng lặp
injectVerifiedBadge()
showNotification()

-- Chạy liên tục để bắt các khung tên trên bảng xếp hạng (khi mở bảng Tab hoặc khi player load)
task.spawn(function()
    while true do
        injectVerifiedBadge()
        task.wait(0.2) -- Tần suất quét để không bị giật lag
    end
end)

print("Đã bật và chèn sẵn ký tự tích xanh  vào bảng xếp hạng thực thành công!")
