local drawing = {Objects = {}}
getgenv().Objects = drawing.Objects
local maths = {}
local Lib = {}

function drawing:NewDrawing(player, type)
    local _Drawing = Drawing.new(type)
    drawing.Objects[player] = _Drawing
    return _Drawing
end

function drawing:GetDrawing(player) -- probably not needed
    return drawing.Objects[player]
end 

-- Get 4 corners in vector3 format that if lines were drawn between each it would form a rectangle
maths.GetCorners = function(Object) 
    local Position = Object.Position
    local Size = Object.Size
    local TopX, TopY = Position.X - (Size.X / 2), Position.Y - (Size.Y / 2)
    local BottomY = TopY - Size.Y

    return {
        TopLeft = Vector3.new(TopX, TopY, Position.Z),
		TopRight = Vector3.new(TopX + Size.X, TopY, Position.Z),
		BottomLeft = Vector3.new(TopX, BottomY, Position.Z),
		BottomRight = Vector3.new(TopX + Size.X, BottomY, Position.Z)
    } 
end

maths.GetClosestWithin = function(CompareFrom, CompareTo, TargetDistance)
    local Distance = TargetDistance or math.huge
    local Closest
    
    local Dist = (CompareFrom.Position - CompareTo.Position).Magnitude
    if Dist < Distance then
        Distance = Dist
        Closest = CompareTo
    end
    return Closest
end




return {
    Drawings = drawing,
    maths = maths,
   -- Hooks = loadstring(game:HttpGet("https://raw.githubusercontent.com/Cattoware/main/main/libaries/hooks.lua"))(),
}
