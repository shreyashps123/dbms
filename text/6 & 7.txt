/*Write a Stored Procedure namely proc_Grade for the categorization of student. If marks scored by students in
examination is <=1500 and marks>=990 then student will be placed in distinction category if marks scored are
between 989 and900 category is first class, if marks 899 and 825 category is Higher Second Class Write a PL/SQL
block for using procedure created with above requirement. Stud_Marks(name, total_marks) Result(Roll,Name,
Class).*/
-- Create the table to store student marks
CREATE TABLE Stud_Marks (
  Roll NUMBER,
  Name VARCHAR2(100),
  Total_Marks NUMBER
);

-- Create the procedure to categorize students
CREATE OR REPLACE PROCEDURE proc_Grade (
  p_Name IN Stud_Marks.Name%TYPE,
  p_Total_Marks IN Stud_Marks.Total_Marks%TYPE,
  p_Result OUT SYS_REFCURSOR
) AS
  v_Class VARCHAR2(100);
BEGIN
  IF p_Total_Marks <= 1500 AND p_Total_Marks >= 990 THEN
  v_Class := 'Distinction';
  ELSIF p_Total_Marks <= 989 AND p_Total_Marks >= 900 THEN
  v_Class := 'First Class';
  ELSIF p_Total_Marks <= 899 AND p_Total_Marks >= 825 THEN
  v_Class := 'Higher Second Class';
  ELSE
  v_Class := 'Not Categorized';
  END IF;

  -- Insert the result into the Result table
  INSERT INTO Result (Roll, Name, Class)
  VALUES (Roll_seq.NEXTVAL, p_Name, v_Class);

  -- Return the result as a cursor
  OPEN p_Result FOR
  SELECT Roll, Name, Class
  FROM Result;
END;
/

-- Declare variables
DECLARE
  v_Result SYS_REFCURSOR;
BEGIN
  -- Call the procedure for each student
  proc_Grade('John Doe', 1450, v_Result);
  proc_Grade('Jane Smith', 950, v_Result);
  proc_Grade('Michael Johnson', 875, v_Result);

  -- Print the result
  DBMS_OUTPUT.PUT_LINE('Roll | Name           | Class');
  DBMS_OUTPUT.PUT_LINE('-----------------------------');
  LOOP
  FETCH v_Result INTO Roll, Name, Class;
  EXIT WHEN v_Result%NOTFOUND;
  DBMS_OUTPUT.PUT_LINE(Roll || ' | ' || Name || ' | ' || Class);
  END LOOP;
  CLOSE v_Result;
END;
/