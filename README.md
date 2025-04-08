using Microsoft.EntityFrameworkCore;
using System.Text.Json.Serialization;
using System.Globalization;

namespace LeaveReasonSystem.Models
{

[Keyless]
public record LeaveReasonInfo
{
    public int RowNumber { get; set; }
    public int Person_Id { get; set; }

    [JsonIgnore]
    private int Actual_Termination_Date { get; set; }
    public string Actual_Termination_Date_Raw =>
        DateTime.ParseExact(
            Actual_Termination_Date_Raw.ToString(),
             "yyyyMMdd",
             CultureInfo.InvariantCulture
             ).ToString("dd.MM.yyyy");
    public string Full_Name_Local { get; set; } = string.Empty;

    
    

}

}




PS C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App> dotnet run
Using launch settings from C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App\Properties\launchSettings.json...
Building...
C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App\services\LeaveReasonService.cs(26,17): error CS0122: 'LeaveReasonInfo.Actual_Termination_Date' is inaccessible due to its protection level
C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App\services\LeaveReasonService.cs(26,50): error CS0122: 'LeaveReasonInfo.Actual_Termination_Date' is inaccessible due to its protection level
C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App\services\LeaveReasonService.cs(30,20): warning CS8619: Nullability of reference types in value of type '?' doesn't match target type 'List<LeaveReasonInfo>'.

The build failed. Fix the build errors and run again.
