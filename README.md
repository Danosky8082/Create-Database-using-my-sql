# Create-Database-using-my-sql
Design and implement a full-featured database using only MySQL.

-- Drop tables if they already exist (for reset)
DROP TABLE IF EXISTS Prescription_Medications;
DROP TABLE IF EXISTS Prescriptions;
DROP TABLE IF EXISTS Appointments;
DROP TABLE IF EXISTS Doctor_Specializations;
DROP TABLE IF EXISTS Medications;
DROP TABLE IF EXISTS Specializations;
DROP TABLE IF EXISTS Doctors;
DROP TABLE IF EXISTS Patients;

-- Patients Table
CREATE TABLE Patients (
    patient_id INT AUTO_INCREMENT PRIMARY KEY,
    full_name VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    phone VARCHAR(15),
    date_of_birth DATE,
    gender ENUM('Male', 'Female', 'Other') NOT NULL
);

-- Doctors Table
CREATE TABLE Doctors (
    doctor_id INT AUTO_INCREMENT PRIMARY KEY,
    full_name VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    phone VARCHAR(15),
    license_number VARCHAR(50) NOT NULL UNIQUE
);

-- Specializations Table
CREATE TABLE Specializations (
    specialization_id INT AUTO_INCREMENT PRIMARY KEY,
    specialization_name VARCHAR(100) NOT NULL UNIQUE
);

-- Doctor_Specializations (Many-to-Many)
CREATE TABLE Doctor_Specializations (
    doctor_id INT NOT NULL,
    specialization_id INT NOT NULL,
    PRIMARY KEY (doctor_id, specialization_id),
    FOREIGN KEY (doctor_id) REFERENCES Doctors(doctor_id) ON DELETE CASCADE,
    FOREIGN KEY (specialization_id) REFERENCES Specializations(specialization_id) ON DELETE CASCADE
);

-- Appointments Table
CREATE TABLE Appointments (
    appointment_id INT AUTO_INCREMENT PRIMARY KEY,
    patient_id INT NOT NULL,
    doctor_id INT NOT NULL,
    appointment_date DATETIME NOT NULL,
    status ENUM('Scheduled', 'Completed', 'Cancelled') NOT NULL DEFAULT 'Scheduled',
    FOREIGN KEY (patient_id) REFERENCES Patients(patient_id) ON DELETE CASCADE,
    FOREIGN KEY (doctor_id) REFERENCES Doctors(doctor_id) ON DELETE CASCADE
);

-- Prescriptions Table (One-to-One with Appointment)
CREATE TABLE Prescriptions (
    prescription_id INT AUTO_INCREMENT PRIMARY KEY,
    appointment_id INT NOT NULL UNIQUE,
    notes TEXT,
    issued_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (appointment_id) REFERENCES Appointments(appointment_id) ON DELETE CASCADE
);

-- Medications Table
CREATE TABLE Medications (
    medication_id INT AUTO_INCREMENT PRIMARY KEY,
    medication_name VARCHAR(100) NOT NULL UNIQUE,
    dosage VARCHAR(50) NOT NULL
);

-- Prescription_Medications (Many-to-Many)
CREATE TABLE Prescription_Medications (
    prescription_id INT NOT NULL,
    medication_id INT NOT NULL,
    PRIMARY KEY (prescription_id, medication_id),
    FOREIGN KEY (prescription_id) REFERENCES Prescriptions(prescription_id) ON DELETE CASCADE,
    FOREIGN KEY (medication_id) REFERENCES Medications(medication_id) ON DELETE CASCADE
);

