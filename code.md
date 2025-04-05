# создание таблиц:

-- Создание таблицы Клиенты
CREATE TABLE Clients (
    client_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100),
    phone VARCHAR(15),
    date_of_birth DATE,
    registration_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Создание таблицы Номера
CREATE TABLE Rooms (
    room_id SERIAL PRIMARY KEY,
    room_number VARCHAR(10),
    room_type VARCHAR(50),
    capacity INT,
    price_per_night DECIMAL(10, 2),
    status VARCHAR(20) CHECK (status IN ('available', 'occupied', 'maintenance'))
);

-- Создание таблицы Бронирования
CREATE TABLE Bookings (
    booking_id SERIAL PRIMARY KEY,
    client_id INT,
    room_id INT,
    check_in_date DATE,
    check_out_date DATE,
    booking_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    status VARCHAR(20) CHECK (status IN ('confirmed', 'canceled', 'completed')),
    FOREIGN KEY (client_id) REFERENCES Clients(client_id),
    FOREIGN KEY (room_id) REFERENCES Rooms(room_id)
);

-- Создание таблицы Услуги
CREATE TABLE Services (
    service_id SERIAL PRIMARY KEY,
    service_name VARCHAR(100),
    price DECIMAL(10, 2)
);

-- Создание таблицы Платежи
CREATE TABLE Payments (
    payment_id SERIAL PRIMARY KEY,
    booking_id INT,
    amount DECIMAL(10, 2),
    payment_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    payment_method VARCHAR(20) CHECK (payment_method IN ('cash', 'credit_card', 'bank_transfer')),
    FOREIGN KEY (booking_id) REFERENCES Bookings(booking_id)
);


# заполнение данными:

-- Заполнение таблицы Rooms (Комнаты)
INSERT INTO Rooms (room_number, room_type, price_per_night, status)
VALUES
('101', 'Стандарт', 2500.00, 'available'),
('102', 'Стандарт', 2500.00, 'available'),
('201', 'Люкс', 5000.00, 'available'),
('202', 'Люкс', 5000.00, 'maintenance'),
('301', 'Президентский', 10000.00, 'available');

-- Заполнение таблицы Clients (Клиенты)
INSERT INTO Clients (last_name, first_name, phone_number, email, passport_number)
VALUES
('Иванов', 'Иван', '+79161234567', 'ivanov@mail.ru', '1234567890'),
('Петрова', 'Мария', '+79262345678', 'petrova@gmail.com', '2345678901'),
('Сидоров', 'Алексей', '+79373456789', NULL, '3456789012'),
('Кузнецова', 'Елена', '+79484567890', 'kuznetsova@yandex.ru', '4567890123');

-- Заполнение таблицы Reservations (Бронирования)
INSERT INTO Reservations (client_id, room_id, check_in_date, check_out_date, status, total_price)
VALUES
(1, 1, '2023-11-15', '2023-11-20', 'confirmed', 12500.00),
(2, 3, '2023-11-18', '2023-11-22', 'confirmed', 20000.00),
(3, 2, '2023-11-20', '2023-11-25', 'cancelled', 12500.00),
(4, 5, '2023-12-01', '2023-12-10', 'confirmed', 90000.00);

-- Заполнение таблицы Stays (Проживания)
INSERT INTO Stays (reservation_id, client_id, room_id, actual_check_in, actual_check_out, status)
VALUES
(1, 1, 1, '2023-11-15 14:00:00', '2023-11-20 12:00:00', 'checked-out'),
(2, 2, 3, '2023-11-18 15:30:00', NULL, 'in-house'),
(4, 4, 5, NULL, NULL, 'in-house');

-- Заполнение таблицы Payments (Платежи)
INSERT INTO Payments (stay_id, amount, payment_date, payment_method)
VALUES
(1, 5000.00, '2023-11-10 10:15:00', 'card'),
(1, 7500.00, '2023-11-19 16:30:00', 'cash'),
(2, 10000.00, '2023-11-15 11:20:00', 'card'),
(3, 30000.00, '2023-11-25 14:45:00', 'card');
