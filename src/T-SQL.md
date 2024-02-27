##### 1. Template: Stored procedure with transaction
```sql
CREATE OR ALTER PROC [dbo].[uspAStoredProcedure]
    @anId INT,
    @anAttribute NVARCHAR(100),
    @aResult INT OUTPUT
AS
BEGIN
    SET NOCOUNT ON;
    BEGIN TRAN uspAStoredProcedure_T1;

    BEGIN TRY
        -- CRUD Ops

        SET @aResult = 1;
        COMMIT TRAN uspAStoredProcedure_T1;
    END TRY
    BEGIN CATCH
        ROLLBACK TRAN uspAStoredProcedure_T1;
        THROW;
    TRY CATCH
END
GO
```
