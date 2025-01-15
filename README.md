import React, { useState, useEffect } from 'react';
import { Button } from '@/components/ui/button';
import { Card } from '@/components/ui/card';
import { Input } from '@/components/ui/input';
import { Textarea } from '@/components/ui/textarea';
import { 
  Shield, 
  Book, 
  Award, 
  Briefcase, 
  Download,
  Code,
  Target,
  Users,
  Server
} from 'lucide-react';

export default function Portfolio() {
  const [selectedImage, setSelectedImage] = useState(null);
  const [selectedCV, setSelectedCV] = useState(null);
  const [matrixChars, setMatrixChars] = useState([]);

  // Efecto para el fondo de matriz
  useEffect(() => {
    const chars = '01アイウエオカキクケコサシスセソタチツテトナニヌネノハヒフヘホマミムメモヤユヨラリルレロワヲン';
    const columns = Math.floor(window.innerWidth / 20);
    const initialChars = Array.from({ length: columns * 5 }, () => ({
      char: chars[Math.floor(Math.random() * chars.length)],
      x: Math.floor(Math.random() * columns) * 20,
      y: Math.random() * -1000,
      speed: 1 + Math.random() * 2,
      opacity: Math.random()
    }));
    
    setMatrixChars(initialChars);

    const interval = setInterval(() => {
      setMatrixChars(prev => prev.map(char => ({
        ...char,
        y: char.y > window.innerHeight ? -20 : char.y + char.speed,
        char: Math.random() < 0.01 ? chars[Math.floor(Math.random() * chars.length)] : char.char,
        opacity: Math.random() < 0.1 ? Math.random() : char.opacity
      })));
    }, 50);

    return () => clearInterval(interval);
  }, []);

  const experienceData = [
    {
      company: "Empresa de Ciberseguridad A",
      position: "Senior Security Analyst",
      period: "2020 - Presente",
      responsibilities: [
        "Implementación de SIEM empresarial",
        "Gestión de incidentes de seguridad",
        "Auditorías de seguridad"
      ]
    },
    {
      company: "Consultora de Seguridad B",
      position: "Security Consultant",
      period: "2018 - 2020",
      responsibilities: [
        "Pentesting de aplicaciones web",
        "Evaluaciones de vulnerabilidades",
        "Hardening de sistemas"
      ]
    }
  ];

  const skills = [
    { category: "Seguridad Ofensiva", items: ["Pentesting", "Ethical Hacking", "OSINT", "Vulnerability Assessment"] },
    { category: "Seguridad Defensiva", items: ["SIEM", "EDR", "Incident Response", "Threat Hunting"] },
    { category: "Cloud Security", items: ["AWS Security", "Azure Security", "Cloud Architecture", "IAM"] },
    { category: "Compliance", items: ["ISO 27001", "NIST", "GDPR", "PCI DSS"] }
  ];

  return (
    <div className="min-h-screen bg-gray-900 relative">
      {/* Fondo de Matrix */}
      <div className="fixed inset-0 overflow-hidden pointer-events-none">
        {matrixChars.map((char, i) => (
          <div
            key={i}
            className="absolute text-green-500 text-opacity-50 font-matrix"
            style={{
              left: char.x,
              top: char.y,
              opacity: char.opacity,
              transform: 'scale(1.5)',
              transition: 'opacity 0.5s'
            }}
          >
            {char.char}
          </div>
        ))}
      </div>

      {/* Contenido Principal */}
      <div className="relative z-10 p-8">
        {/* Header con foto y CV */}
        <div className="max-w-6xl mx-auto bg-gray-800/90 rounded-lg p-8 mb-8 backdrop-blur-sm">
          <div className="flex flex-col md:flex-row items-center gap-8">
            <div className="w-48 h-48 bg-gray-700 rounded-full overflow-hidden border-4 border-blue-500/30">
              {selectedImage ? (
                <img 
                  src={selectedImage} 
                  alt="Profile" 
                  className="w-full h-full object-cover"
                />
              ) : (
                <div className="w-full h-full flex items-center justify-center">
                  <input
                    type="file"
                    accept="image/*"
                    onChange={(e) => setSelectedImage(URL.createObjectURL(e.target.files[0]))}
                    className="hidden"
                    id="photo-upload"
                  />
                  <label 
                    htmlFor="photo-upload"
                    className="cursor-pointer text-gray-400 hover:text-white text-center p-4"
                  >
                    Click para subir foto
                  </label>
                </div>
              )}
            </div>
            
            <div className="flex-1">
              <h1 className="text-4xl font-bold text-white mb-2">
                Victor Eduardo Resendiz Villegas
              </h1>
              <p className="text-2xl text-blue-400 mb-4">
                Especialista en Ciberseguridad & Ethical Hacking
              </p>
              <div className="flex gap-4 mb-4">
                <input
                  type="file"
                  accept=".pdf"
                  onChange={(e) => setSelectedCV(e.target.files[0])}
                  className="hidden"
                  id="cv-upload"
                />
                <label htmlFor="cv-upload">
                  <Button variant="outline" className="cursor-pointer">
                    Subir CV (PDF)
                  </Button>
                </label>
                {selectedCV && (
                  <Button variant="default" className="bg-blue-600 hover:bg-blue-700">
                    <Download className="w-4 h-4 mr-2" />
                    Descargar CV
                  </Button>
                )}
              </div>
              <p className="text-gray-300">
                Especialista en ciberseguridad con más de 7 años de experiencia en protección de infraestructuras críticas,
                respuesta a incidentes y consultoría de seguridad para empresas Fortune 500.
              </p>
            </div>
          </div>
        </div>

        {/* Grid de contenido principal */}
        <div className="max-w-6xl mx-auto grid grid-cols-1 md:grid-cols-2 gap-8">
          {/* Experiencia */}
          <Card className="bg-gray-800/90 p-6 backdrop-blur-sm">
            <div className="flex items-center gap-2 mb-6">
              <Briefcase className="w-6 h-6 text-blue-400" />
              <h2 className="text-2xl font-bold text-white">Experiencia Profesional</h2>
            </div>
            <div className="space-y-6">
              {experienceData.map((exp, index) => (
                <div key={index} className="border-l-2 border-blue-500 pl-4">
                  <h3 className="text-xl font-semibold text-white">{exp.company}</h3>
                  <p className="text-blue-400">{exp.position}</p>
                  <p className="text-gray-400 text-sm">{exp.period}</p>
                  <ul className="mt-2 space-y-1">
                    {exp.responsibilities.map((resp, i) => (
                      <li key={i} className="text-gray-300">• {resp}</li>
                    ))}
                  </ul>
                </div>
              ))}
            </div>
          </Card>

          {/* Habilidades */}
          <Card className="bg-gray-800/90 p-6 backdrop-blur-sm">
            <div className="flex items-center gap-2 mb-6">
              <Code className="w-6 h-6 text-blue-400" />
              <h2 className="text-2xl font-bold text-white">Habilidades Técnicas</h2>
            </div>
            <div className="space-y-6">
              {skills.map((skillGroup, index) => (
                <div key={index}>
                  <h3 className="text-lg font-semibold text-blue-400 mb-2">
                    {skillGroup.category}
                  </h3>
                  <div className="flex flex-wrap gap-2">
                    {skillGroup.items.map((skill, i) => (
                      <span 
                        key={i}
                        className="bg-gray-700 text-gray-300 px-3 py-1 rounded-full text-sm"
                      >
                        {skill}
                      </span>
                    ))}
                  </div>
                </div>
              ))}
            </div>
          </Card>

          {/* Logros y Métricas */}
          <Card className="bg-gray-800/90 p-6 backdrop-blur-sm">
            <div className="flex items-center gap-2 mb-6">
              <Target className="w-6 h-6 text-blue-400" />
              <h2 className="text-2xl font-bold text-white">Logros Destacados</h2>
            </div>
            <div className="grid grid-cols-2 gap-4">
              <div className="text-center p-4 bg-gray-700/50 rounded-lg">
                <h3 className="text-3xl font-bold text-blue-400">50+</h3>
                <p className="text-gray-300">Vulnerabilidades críticas identificadas</p>
              </div>
              <div className="text-center p-4 bg-gray-700/50 rounded-lg">
                <h3 className="text-3xl font-bold text-blue-400">100%</h3>
                <p className="text-gray-300">Tasa de resolución de incidentes</p>
              </div>
              <div className="text-center p-4 bg-gray-700/50 rounded-lg">
                <h3 className="text-3xl font-bold text-blue-400">30+</h3>
                <p className="text-gray-300">Empresas asesoradas</p>
              </div>
              <div className="text-center p-4 bg-gray-700/50 rounded-lg">
                <h3 className="text-3xl font-bold text-blue-400">24/7</h3>
                <p className="text-gray-300">Monitoreo de seguridad implementado</p>
              </div>
            </div>
          </Card>

          {/* Certificaciones */}
          <Card className="bg-gray-800/90 p-6 backdrop-blur-sm">
            <div className="flex items-center gap-2 mb-6">
              <Award className="w-6 h-6 text-blue-400" />
              <h2 className="text-2xl font-bold text-white">Certificaciones</h2>
            </div>
            <div className="grid grid-cols-1 gap-4">
              <div className="flex items-center gap-4 p-4 bg-gray-700/50 rounded-lg">
                <Shield className="w-12 h-12 text-blue-400" />
                <div>
                  <h3 className="text-lg font-semibold text-white">CISSP</h3>
                  <p className="text-gray-400">ISC² - 2023</p>
                  <input
                    type="file"
                    accept=".pdf"
                    className="hidden"
                    id="cert-upload-1"
                  />
                  <label 
                    htmlFor="cert-upload-1"
                    className="text-sm text-blue-400 hover:text-blue-300 cursor-pointer"
                  >
                    Subir certificado
                  </label>
                </div>
              </div>
              <Button 
                variant="outline" 
                className="w-full border-dashed border-2 border-gray-600 hover:border-blue-400"
              >
                + Agregar certificación
              </Button>
            </div>
          </Card>
        </div>

        {/* Footer con información de contacto */}
        <footer className="max-w-6xl mx-auto mt-8 bg-gray-800/90 rounded-lg p-6 backdrop-blur-sm">
          <div className="grid grid-cols-1 md:grid-cols-3 gap-6">
            <div className="text-center">
              <Server className="w-6 h-6 text-blue-400 mx-auto mb-2" />
              <h3 className="text-lg font-semibold text-white mb-2">Ubicación</h3>
              <p className="text-gray-300">Ciudad de México, México</p>
            </div>
            <div className="text-center">
              <Users className="w-6 h-6 text-blue-400 mx-auto mb-2" />
              <h3 className="text-lg font-semibold text-white mb-2">Redes Profesionales</h3>
              <div className="space-y-1">
                <a href="#" className="text-blue-400 hover:text-blue-300 block">LinkedIn</a>
                <a href="#" className="text-blue-400 hover:text-blue-300 block">GitHub</a>
              </div>
            </div>
            <div className="text-center">
              <Shield className="w-6 h-6 text-blue-400 mx-auto mb-2" />
              <h3 className="text-lg font-semibold text-white mb-2">Contacto Directo</h3>
              <p className="text-gray-300">contacto@ejemplo.com</p>
            </div>
          </div>
        </footer>
      </div>
    </div>
  );
}
