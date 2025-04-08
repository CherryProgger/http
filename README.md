	select distinct 
	CAST((ROW_NUMBER() OVER (ORDER BY Person_Id)) AS INT) AS RowNumber
	,vasg.Person_Id
	,CAST(FORMAT(vasg.Actual_Termination_Date, 'yyyyMMdd') AS INT) AS vasg.Actual_Termination_Date
	,vasg.Full_Name_Local
	from HRPowerTool_DWH.dbo.vw_A_ASSIGNMENT_decode vasg
