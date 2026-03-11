# 2400032544-FSAD-IN-SEM
package com.klef.fsad.exam;

import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.Transaction;
import org.hibernate.cfg.Configuration;

import java.util.Date;

public class ClientDemo {
    public static void main(String[] args) {
        // Load Hibernate configuration
        Configuration cfg = new Configuration();
        cfg.configure("hibernate.cfg.xml");
        SessionFactory factory = cfg.buildSessionFactory();

        // Insert a new record
        insertVehicle(factory);

        // Update record by ID
        updateVehicle(factory, 1, "Updated Car Name", "Active");

        factory.close();
    }

    // Insert operation
    public static void insertVehicle(SessionFactory factory) {
        Session session = factory.openSession();
        Transaction tx = session.beginTransaction();

        Vehicle v = new Vehicle("Car", "Four wheeler vehicle", new Date(), "Available");
        session.save(v);

        tx.commit();
        session.close();
        System.out.println("Vehicle inserted successfully with ID: " + v.getId());
    }

    // Update operation
    public static void updateVehicle(SessionFactory factory, int id, String newName, String newStatus) {
        Session session = factory.openSession();
        Transaction tx = session.beginTransaction();

        Vehicle v = session.get(Vehicle.class, id);
        if (v != null) {
            v.setName(newName);
            v.setStatus(newStatus);
            session.update(v);
            System.out.println("Vehicle updated successfully!");
        } else {
            System.out.println("Vehicle not found with ID: " + id);
        }

        tx.commit();
        session.close();
    }
}
