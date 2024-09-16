SELECT
  X.a,
  X.b,
  X.c,
  X.d,
  X.e,
  Y.i,
  Y.j,
  (X.c / Y.i) AS c_divided_by_i,
  (X.b * Y.j) AS b_multiplied_by_j
FROM
  `project.dataset.X` AS X
JOIN
  `project.dataset.Y` AS Y
ON
  X.d = Y.d AND X.e = Y.e
