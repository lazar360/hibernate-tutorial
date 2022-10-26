# hibernate-tutorial

public class QueryStudentDemo {

	@SuppressWarnings("unchecked")
	public static void main(String[] args) {

		// create session factory
		SessionFactory factory = new Configuration()
								.configure("hibernate.cfg.xml")
								.addAnnotatedClass(Student.class)
								.buildSessionFactory();
		
		// create session 
		Session session = factory.getCurrentSession();
		
		try {	
			// start a transaction
			session.beginTransaction(); 
			
			//query students
			List<Student> theStudents = session.createQuery("from Student").getResultList();
			
			// display the students
			displayStudents(theStudents);
			
			// query students : lastName = 'Doe' / use the java property name not column name
			theStudents = session.createQuery("from Student s where s.lastName ='Doe'").getResultList();
			
			//display the students
			System.out.println("\n\n Students who have last name of Doe : ");
			displayStudents(theStudents);
			
			// query students : lastName = 'Doe' OR firstName = 'Daffy'/ use the java property name not column name
			theStudents = session.createQuery("from Student s where" + " s.lastName ='Doe' OR s.firstName ='Daffy'").getResultList();
			
			//display the students
			System.out.println("\n\n Students who have last name of Doe OR first name of Daffy: ");
			displayStudents(theStudents);
			
			// query students where email LIKE '%luve2code.com'
			theStudents = session.createQuery("from Student s where" + " s.email LIKE '%luv2code.com'").getResultList();
			
			//display the students
			System.out.println("\n\n Students who have email ends with luv2code.com : ");
			displayStudents(theStudents);
			
			// commit transaction
			session.getTransaction().commit();
			System.out.println("Done !");
			
		} finally {
			factory.close();
		}
		
	}

	private static void displayStudents(List<Student> theStudents) {
		for (Student tempStudent : theStudents) {
			System.out.println(tempStudent);
		}
	}
