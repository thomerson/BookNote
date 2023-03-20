## 左填充

```sql
ALTER FUNCTION [dbo].[F_LeftPad]
(
    @inStr nvarchar(max),
    @inLen int,
    @inPadChar char(1)
)
returns nvarchar(max)
AS
BEGIN

    RETURN REPLICATE(@inPadChar,@inLen-LEN(@inStr)) + @inStr
END

```

## 右填充

```sql
ALTER FUNCTION [dbo].[F_RightPad]
(
    @inStr nvarchar(max),
    @inLen int,
    @inPadChar char(1)
)
returns nvarchar(max)
AS
BEGIN

    RETURN @inStr + REPLICATE(@inPadChar,@inLen-LEN(@inStr)) 
END

```