# 🏫 Hệ Thống Quản Lý Phòng Học Bộ Môn (PBM)

> Web application quản lý phòng học bộ môn - Trường Đại học/THPT

## 📋 Mô Tả

Ứng dụng web quản lý 7 phòng học bộ môn với đầy đủ chức năng:
- **Phòng BM Tiếng Anh**
- **Phòng THTN Sinh**
- **Phòng THTN Lý**
- **Phòng THTN Hoá**
- **Phòng Tin học 1**
- **Phòng Tin học 2**
- **Phòng thiết bị dùng chung**

## 👥 Người Dùng & Phân Quyền

| Role | Số Lượng | Chức Năng Chính |
|------|----------|-----------------|
| **Admin** | 1 | Quản lý toàn hệ thống, tạo BGH, phân quyền |
| **BGH** | 3 | Quản trị viên cấp trường, hỗ trợ Admin |
| **GV Manager** | 7+ | Quản lý phòng BM được giao (thiết bị, báo cáo, GV) |
| **GV Staff** | N | Đăng ký sử dụng phòng, cập nhật tình trạng |

## ✨ Tính Năng Chính

### 📊 Quản Lý Tài Khoản
- ✅ Tạo/Sửa/Xóa tài khoản (Admin)
- ✅ Phân quyền GV quản lý phòng
- ✅ Quản lý trạng thái tài khoản (Active/Inactive)

### 🏛️ Quản Lý Phòng Học
- ✅ Danh sách 7 phòng với trạng thái
- ✅ Gán GV quản lý cho mỗi phòng
- ✅ Quản lý GV bộ môn được phép sử dụng
- ✅ Cập nhật tình trạng phòng

### 🖥️ Quản Lý Thiết Bị
- ✅ Danh sách thiết bị từng phòng
- ✅ Cập nhật trạng thái (Tốt, Hư, Bảo trì, Mất)
- ✅ Theo dõi bảo hành
- ✅ Báo cáo thiết bị có vấn đề

### 📅 Đăng Ký & Lịch
- ✅ GV bộ môn đăng ký sử dụng phòng (Ngày, Tiết, Lớp)
- ✅ Ghi nhận tình trạng trước/sau tiết
- ✅ GV Manager duyệt đơn
- ✅ Xem lịch chung các phòng

### 📈 Thống Kê & Báo Cáo
- ✅ Thống kê lượt sử dụng phòng
- ✅ Danh sách GV sử dụng nhiều nhất
- ✅ Báo cáo thiết bị
- ✅ Xuất file Excel, PDF

## 🛠️ Tech Stack

### Frontend
- **Framework**: React.js / Next.js
- **Styling**: TailwindCSS / Material-UI
- **State Management**: React Query
- **Charts**: Chart.js / Apache ECharts
- **Calendar**: React Calendar
- **HTTP**: Axios

### Backend
- **Runtime**: Node.js
- **Framework**: Express.js
- **Database**: MongoDB / PostgreSQL
- **Authentication**: JWT
- **Validation**: Express Validator
- **Export**: ExcelJS, jsPDF

### DevOps
- **Containerization**: Docker
- **Deployment**: Vercel (Frontend), Heroku/Railway (Backend)
- **Database**: MongoDB Atlas
- **Version Control**: Git

## 📁 Cấu Trúc Thư Mục

```
pbm/
├── docs/                      # Tài liệu dự án
│   ├── DESIGN.md             # Thiết kế hệ thống
│   ├── API_SPECS.md          # Các API endpoints
│   └── DATABASE_SCHEMA.md    # Schema cơ sở dữ liệu
├── frontend/                  # React Frontend
│   ├── src/
│   │   ├── components/       # Reusable components
│   │   ├── pages/           # Page components
│   │   ├── services/        # API services
│   │   ├─��� hooks/           # Custom hooks
│   │   ├── styles/          # Global styles
│   │   └── utils/           # Utilities
│   ├── public/              # Static assets
│   └── package.json
├── backend/                   # Express Backend
│   ├── src/
│   │   ├── routes/          # API routes
│   │   ├── controllers/      # Business logic
│   │   ├── models/          # Database models
│   │   ├── middleware/      # Authentication, validation
│   │   ├── config/          # Configuration
│   │   └── utils/           # Utilities
│   ├── tests/               # Unit & integration tests
│   └── package.json
├── tests/                     # E2E tests
├── .gitignore
├── README.md                 # File này
└── docker-compose.yml       # Docker configuration

```

## 🚀 Hướng Dẫn Cài Đặt

### Yêu Cầu
- Node.js 16+
- MongoDB / PostgreSQL
- Git

### Backend Setup
```bash
cd backend
npm install
cp .env.example .env
npm start
```

### Frontend Setup
```bash
cd frontend
npm install
npm start
```

## 📖 Tài Liệu

- [Thiết Kế Hệ Thống](./docs/DESIGN.md)
- [API Specifications](./docs/API_SPECS.md)
- [Database Schema](./docs/DATABASE_SCHEMA.md)

## 👨‍💻 Phát Triển

### Branch Strategy
- `main` - Production
- `develop` - Development
- `feature/*` - Feature branches
- `bugfix/*` - Bug fixes

### Commit Convention
```
[FEATURE] Add room booking
[BUGFIX] Fix login error
[DOCS] Update README
[REFACTOR] Optimize API
```

## 📝 License

© 2026 Trường Đại học/THPT. All rights reserved.

## 📞 Liên Hệ

**Project Manager**: [Your Name]
**Email**: contact@school.edu.vn
**Support**: support@school.edu.vn

---

**Last Updated**: 2026-04-10
