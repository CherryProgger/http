using System.Text.Json.Serialization;

public class LeaveReasonRecord
{
    [JsonPropertyName("personId")]
    public int PersonId { get; set; }

    [JsonPropertyName("idLeaveReason")]
    public string IdLeaveReason { get; set; } = string.Empty;
}
