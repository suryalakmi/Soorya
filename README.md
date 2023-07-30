employeee
service.java
package com.example.Employee.service;
import java.util.List;



import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.example.Employee.domain.Employee;
import com.example.Employee.repository.EmpRepository;
import com.example.Employee.service.EmpService;


	@Service
	public class EmpService {
		
		 @Autowired
		private EmpRepository repo;
		 public List<Employee>listAll(){
			 return repo.findAll();
			 
	}
		 public void save(Employee emp) {
			 repo.save(emp);
		 }
		 public  Employee get(long id) {
			 return (Employee) repo.findById(id).get();
		 }
		 public void delete(long id) {
			 repo.deleteById(id);
		 }

   
controller.java
package com.example.Employee.controller;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.servlet.ModelAndView;

import com.example.Employee.domain.Employee;
import com.example.Employee.service.EmpService;

@Controller
public class EmpController {
	@Autowired
    private EmpService service;

    @GetMapping("/")
    public String viewHomePage(Model model) {
        List<Employee> listemployee = service.listAll();
        model.addAttribute("listemployee", listemployee);
        System.out.print("Get / ");	
        return "index";
    }

    @GetMapping("/new")
    public String add(Model model) {
        model.addAttribute("employee", new Employee());
        return "new";
    }

    @RequestMapping(value = "/save", method = RequestMethod.POST)
    public String saveEmployee(@ModelAttribute("employee") Employee emp) {
        service.save(emp);
        return "redirect:/";
    }

    @RequestMapping("/edit/{id}")
    public ModelAndView showEditEmployeePage(@PathVariable(name = "id") int id) {
        ModelAndView mav = new ModelAndView("new");
        Employee emp = service.get(id);
        mav.addObject("employee", emp);
        return mav;
        
    }
    @RequestMapping("/delete/{id}")
    public String deleteEmployeePage(@PathVariable(name = "id") int id) {
        service.delete(id);
        return "redirect:/";
}}


package com.example.Employee.domain;


import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
@Entity
public class Employee {
	@Id
    @GeneratedValue(strategy= GenerationType.IDENTITY)
    private Long id;
    private String empname;
    private int mobile;
    private int salary;
public Employee() {
		
	}
	public Employee(Long id, String empname, int mobile, int salary) {
		
		this.id = id;
		this.empname = empname;
		this.mobile = mobile;
		this.salary = salary;
	}
	
	public Long getId() {
		return id;
	}
	public void setId(Long id) {
		this.id = id;
	}
	public String getEmpname() {
		return empname;
	}
	public void setEmpname(String empname) {
		this.empname = empname;
	}
	public int getMobile() {
		return mobile;
	}
	public void setMobile(int mobile) {
		this.mobile = mobile;
	}
	public int getSalary() {
		return salary;
	}
	public void setSalary(int salary) {
		this.salary = salary;
	}
	@Override
	public String toString() {
		return "Employee [id=" + id + ", empname=" + empname + ", mobile=" + mobile + ", salary=" + salary + "]";
	}
}




com.example.gcompany.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

import com.example.gcompany.domain.Employee;

@Repository
public interface EmployeeRepository extends JpaRepository<Employee, Long> {

}



HTML File

html>
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:th="http://www.thymeleaf.org">

<head>
	<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootswatch/4.5.2/cosmo/bootstrap.min.css" />
	<script src= "https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js" ></script>
</head>
<body>
<div>
	<h2 >Spring Boot Crud System -Employee Registation</h2>
	<tr>
		<div align = "left" >
		   
		     <h3><a  th:href="@{'/new'}">Add new</a></h3>  
		   
	    </div>
	
	</tr>
	<tr>
	
	<div class="col-sm-5" align = "center">
                 <div class="panel-body" align = "center" >
                 
   class="table">
  <thead class="thead-dark">
    <tr>
      		<th>Employee ID</th>
            <th>Employee Name</th>
            <th>Mobile</th>
            <th>Salary</th>
            <th>edit</th>
             <th>delete</th>
   	</tr>
  </thead>
  <tbody>
      <tr  th:each="employee : ${listemployee}">
		<td th:text="${employee.id}">Employee ID</td>
		<td th:text="${employee.ename}">Employee Name</td>
		<td th:text="${employee.mobile}">Mobile</td>
		<td th:text="${employee.salary}">Salary</td>				
		<td>
			<a th:href="@{'/edit/' + ${employee.id}}">Edit</a>
		</td>							    
		<td>
			<a th:href="@{'/delete/' + ${employee.id}}">Delete</a>
		</td>		    
		</tr> 
   </div> 

	</tr>

	</tbody>
	</table>
	<div>
</body>
</html>
inside the folder create the another file new.html

<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="utf-8" />
    <title>Create New Product</title>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootswatch/4.5.2/cosmo/bootstrap.min.css" />
    <script src= "https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js" ></script>
</head>
<body>
<div align="center">
    <h1>Create New Employee</h1>
    <br />
     <div class="col-sm-4">
    <form action="#" th:action="@{/save}" th:object="${employee}" method="post">

      
 		<div alight="left">
            <tr>
                <label class="form-label" >Employee Name</label>
                <td>
	                <input type="hidden" th:field="*{id}" />
	                <input type="text" th:field="*{ename}" class="form-control" placeholder="Emp Name" />
                </td>
            </tr>
         </div>   
         alight="left">
            <tr>
                <label class="form-label" >mobile</label>
                
                <td>
                 <input type="text" th:field="*{mobile}" class="form-control" placeholder="mobile" />
                </td>
            </tr>
         </div>  
         <div alight="left">
                 <tr>
                 <label class="form-label" >salary</label>
                <td><input type="text" th:field="*{salary}" class="form-control" placeholder="salary" /></td>
            </tr>
            </div> 
            
			<br>
            <tr>
            <td colspan="2"><button type="submit" class="btn btn-info">Save</button> </td>
            </tr>

    </form>
</div>

</body>
</html>
