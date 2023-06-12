/*
 * To change this license header, choose License Headers in Project Properties.
 * To change this template file, choose Tools | Templates
 * and open the template in the editor.
 */
package dao;

import beans.Dados;
import beans.Usuario;
import conexao.Conexao;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

/**
 *
 * @author thurzin
 */
public class UsuarioDAO {

    private Conexao conexao;
    private Connection conn;

    public UsuarioDAO() {
        this.conexao = new Conexao();
        this.conn = this.conexao.getConexao();
    }

    public ResultSet autenticacao(Usuario usuario) {
        String sq1 = "SELECT * FROM usuario WHERE user=? and senha=?";

        try {
            PreparedStatement ps = this.conn.prepareStatement(sq1);
            ps.setString(1, usuario.getUser());
            ps.setString(2, usuario.getSenha());
            ResultSet rs = ps.executeQuery();
            return rs;
        } catch (SQLException ex) {
            System.err.println("Erro na autenticação, Erro: " + ex);
            return null;
        }
    }

   
}
