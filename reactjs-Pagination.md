# reactjs--Pagination

//App.js

import { useEffect, useState } from "react";
import Pagination from "./Pagination";
import "./styles.css";

export default function App() {
  const [gitHubUsers, setGitHubUsers] = useState([]);
  const [currentPage, setCurrentPage] = useState(1);
  const [tableRowsPerPage, setTableRowsPerPage] = useState(3);

  useEffect(() => {
    fetchUsers();
    getCurrentTableData();
  }, []);

  const fetchUsers = () => {
    fetch("https://api.github.com/users")
      .then((response) => response.json())
      .then((data) => setGitHubUsers(data))
      .catch((error) => console.log(error));
  };

  const getCurrentTableData = () => {
    return gitHubUsers.slice(
      currentPage * tableRowsPerPage - tableRowsPerPage,
      currentPage * tableRowsPerPage
    );
  };

  const paginateData = (pageNumber) => {
    setCurrentPage(pageNumber);
  };

  return (
    <div className="App">
      <table>
        <thead>
          <tr>
            <th>ID</th>
            <th>User</th>
            <th></th>
          </tr>
        </thead>
        <tbody>
          {getCurrentTableData().map((user, index) => (
            <tr key={index}>
              <td>{user.id}</td>
              <td>{user.login}</td>
              <td>
                <img src={user.avatar_url} alt="User avatar" width="50px" />
              </td>
            </tr>
          ))}
        </tbody>
      </table>
      <Pagination
        tableRowsPerPage={tableRowsPerPage}
        totalData={gitHubUsers.length}
        paginateData={paginateData}
      />
    </div>
  );
}
//Pagination.js

import React from "react";

export default function Pagination({
  tableRowsPerPage,
  totalData,
  paginateData
}) {
  const pageNumbers = [];

  for (let i = 1; i < Math.ceil(totalData / tableRowsPerPage); i++) {
    pageNumbers.push(i);
  }

  return (
    <div className="pagination">
      <ul>
        {pageNumbers.map((page, index) => (
          <li key={index} onClick={() => paginateData(page)}>
            {page}
          </li>
        ))}
      </ul>
    </div>
  );
}

